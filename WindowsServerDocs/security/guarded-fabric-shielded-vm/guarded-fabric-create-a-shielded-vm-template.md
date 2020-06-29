---
title: 建立 Windows 受防護的 VM 範本磁片
ms.prod: windows-server
ms.topic: article
ms.assetid: 9c8b84e8-1f5a-47a1-83ca-b1dbd801cb0b
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 01/29/2019
ms.openlocfilehash: 82725e654fb4c7296b092019db111f9d3debad6d
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475385"
---
# <a name="create-a-windows-shielded-vm-template-disk"></a>建立 Windows 受防護的 VM 範本磁片

>適用于： Windows Server （半年通道）、Windows Server 2016、Windows Server 2019


如同一般 Vm，您可以建立 VM 範本（例如， [Virtual Machine Manager （VMM）中的 vm 範本](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-library-add-vm-templates)），讓租使用者和系統管理員可以輕鬆地使用範本磁片在網狀架構上部署新的 vm。 由於受防護的 Vm 是安全性敏感的資產，因此有額外的步驟可建立支援防護的 VM 範本。 本主題涵蓋在 VMM 中建立受防護的範本磁片和 VM 範本的步驟。

若要瞭解本主題如何適用于部署受防護 Vm 的整體程式，請參閱[主機服務提供者設定步驟中的受防護主機和受防護的 vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="prepare-an-operating-system-vhdx"></a>準備作業系統 VHDX

首先準備一個 OS 磁片，然後再透過受防護的範本磁片建立嚮導來執行。 此磁片將用來做為租使用者 Vm 中的 OS 磁片。 您可以使用任何現有的工具來建立此磁片，例如 Microsoft 桌面映射 Service Manager （DISM），或手動設定具有空白 VHDX 的 VM，並將 OS 安裝到該磁片。 設定磁片時，必須遵守下列專屬於第2代和/或受防護 Vm 的需求：

| VHDX 的需求 | 原因 |
|-----------|----|
|必須是 GUID 磁碟分割表格（GPT）磁片 | 第2代虛擬機器需要支援 UEFI|
|磁片類型必須是 [**基本**]，而不是 [**動態**]。 <br>注意：這是指邏輯磁片類型，而不是 Hyper-v 所支援的「動態擴充」 VHDX 功能。 | BitLocker 不支援動態磁碟。|
|磁片至少有兩個磁碟分割。 一個磁碟分割必須包含安裝 Windows 的磁片磁碟機。 此為 BitLocker 將會加密的磁碟機。 另一個磁碟分割是作用中的磁碟分割，其中包含開機載入器，並且會保持未加密狀態，讓電腦可以啟動。|BitLocker 所需|
|檔案系統為 NTFS | BitLocker 所需|
|安裝在 VHDX 上的作業系統是下列其中一項：<br>-Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 <br>-Windows 10、Windows 8.1、Windows 8| 需要支援第2代虛擬機器和 Microsoft 安全開機範本|
|作業系統必須一般化（執行 sysprep.exe） | 範本布建牽涉到將 Vm 專門用於特定租使用者的工作負載|

> [!NOTE]
> 如果您使用 VMM，請勿在此階段將範本磁碟複製到 VMM 程式庫。

## <a name="run-windows-update-on-the-template-operating-system"></a>在範本作業系統上執行 Windows Update

在範本磁片上，確認作業系統已安裝所有最新的 Windows 更新。 最近發行的更新可改善端對端防護程式的可靠性-如果範本作業系統不是最新狀態，則可能無法完成的程式。

## <a name="prepare-and-protect-the-vhdx-with-the-template-disk-wizard"></a>使用範本磁片 wizard 來準備和保護 VHDX

若要使用範本磁片搭配受防護的 Vm，您必須使用受防護的範本磁片建立嚮導，以 BitLocker 備妥磁片並加以加密。 此 wizard 會為磁片產生雜湊，並將它新增至磁片區簽章目錄（VSC）。 VSC 是使用您指定的憑證來簽署，並在布建程式期間使用，以確保租使用者所部署的磁片未遭到變更，或被租使用者不信任的磁片取代。 最後，系統會將 BitLocker 安裝在磁片的作業系統上（如果尚未存在），以便在 VM 布建期間準備磁片進行加密。

> [!NOTE]
> 範本磁片 wizard 會修改您指定的範本磁片。 在執行 wizard 之後，您可能會想要建立未受保護的 VHDX 複本，稍後再更新磁片。 您將無法修改已使用「範本磁片」 wizard 保護的磁片。

在執行 Windows Server 2016、Windows 10 （已安裝遠端伺服器管理工具、RSAT）或更新版本（不需要是受防護主機或 VMM 伺服器）的電腦上執行下列步驟：

1. 將 [[準備作業系統 VHDX](#prepare-an-operating-system-vhdx) ] 中建立的一般化 VHDX 複製到伺服器（如果尚未加入）。

2. 若要在本機管理伺服器，請從伺服器上的**遠端伺服器管理工具**安裝**受防護的 VM 工具**功能。

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart

    您也可以從已安裝[Windows 10 遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)的用戶端電腦管理伺服器。

3. 取得或建立憑證，以簽署將成為新受防護 Vm 之範本磁片的 VHDX VSC。 當租使用者建立其防護資料檔案時，會向租使用者顯示此憑證的詳細資料，並授權其信任的磁片。 因此，請務必從您和您的租使用者雙方信任的憑證授權單位單位取得此憑證。 在您同時是主機服務提供者和租使用者的企業案例中，您可以考慮從 PKI 發行此憑證。

    如果您要設定測試環境，而且只想要使用自我簽署憑證來準備您的範本磁片，請執行類似下列的命令：

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. 從 [開始] 功能表上的 [系統**管理工具**] 資料夾，或在命令提示字元中輸入**TemplateDiskWizard.exe** ，啟動 [**範本磁片]** 。

5. 在 [**憑證**] 頁面上，按一下 **[流覽]** 以顯示憑證的清單。 選取用來準備磁片範本的憑證。 按一下 **[確定]**，然後按 **[下一步]**。

6. 在 [虛擬磁片] 頁面上，按一下 [**流覽]** 以選取您已備妥的 VHDX，然後按 **[下一步]**。

7. 在 [簽章目錄] 頁面上，提供易記的**磁片名稱**和**版本。** 這些欄位已存在，可協助您在備妥磁片後加以識別。

    例如，針對 [**磁片名稱**]，您可以輸入_WS2016_ ，針對**版本**，則為_1.0.0.0_ 。

8. 在嚮導的 [審查設定] 頁面上，檢查您的選擇。 當您按一下 [**產生**] 時，嚮導會在範本磁片上啟用 BitLocker、計算磁片的雜湊，並建立磁片區簽章目錄（儲存在 VHDX 中繼資料中）。

    等到準備程式完成後，再嘗試掛接或移動範本磁片。 視您的磁片大小而定，此程式可能需要一段時間才能完成。

    > [!IMPORTANT]
    > 範本磁片只能與安全受防護的 VM 布建程式搭配使用。
    > 嘗試使用範本磁片來啟動一般（非遮罩） VM 可能會導致停止錯誤（藍色畫面），而且不受支援。

9. 在 [**摘要**] 頁面上，會顯示磁片範本的相關資訊、用來簽署 VSC 的憑證，以及憑證簽發者。 按一下 [關閉] 結束精靈。

如果您使用 VMM，請遵循本主題其餘各節中的步驟，將範本磁片併入 VMM 中受防護的 VM 範本。

## <a name="copy-the-template-disk-to-the-vmm-library"></a>將範本磁碟複製到 VMM 程式庫

如果您使用 VMM，在建立範本磁片之後，您需要將它複製到 VMM 程式庫共用，讓主機可以在布建新的 Vm 時下載並使用該磁片。 請使用下列程式將範本磁碟複製到 VMM 程式庫，然後重新整理程式庫。

1. 將 VHDX 檔案複製到 VMM 程式庫共用資料夾。 如果您使用預設 VMM 設定，請將範本磁碟複製到_ \\ <vmmserver> \MSSCVMMLibrary\VHDs_。

2. 重新整理程式庫伺服器。 開啟 [連結**庫**] 工作區，展開 [連結**庫伺服器**]，在您想要重新整理的程式庫伺服器**上按一下滑鼠**右鍵，然後按一下 [重新整理]。

3. 接下來，提供 VMM 有關範本磁片上所安裝作業系統的資訊：

    a. 在 [連結**庫**] 工作區的程式庫伺服器上，尋找新匯入的範本磁片。

    b. 以滑鼠右鍵按一下磁片，然後按一下 [**屬性**]。

    c. 在 [**作業系統**] 中，展開清單並選取安裝在磁片上的作業系統。 選取作業系統會向 VMM 指出 VHDX 不是空白。

    d. 更新內容之後，按一下 [確定]****。

磁片名稱旁的小型盾牌圖示表示磁片是受防護 Vm 備妥的範本磁片。 您也可以用滑鼠右鍵按一下資料行標頭，並切換**受防護**的資料行，以查看指出磁片是否適用于一般或受防護 VM 部署的文字標記法。

![受防護的 vm 範本磁片](../media/Guarded-Fabric-Shielded-VM/shielded-vm-template-disk.png)

## <a name="create-the-shielded-vm-template-in-vmm-using-the-prepared-template-disk"></a>使用已備妥的範本磁片，在 VMM 中建立受防護的 VM 範本

在 VMM 程式庫中備妥的範本磁片之後，您就可以為受防護的 Vm 建立 VM 範本。 受防護 Vm 的 VM 範本與傳統 VM 範本稍有不同，因為已修正特定設定（第2代 VM、UEFI 和安全開機等），而其他則無法使用（租使用者自訂僅限於少數，請選取 VM 的屬性）。 若要建立 VM 範本，請執行下列步驟：

1. 在 [連結**庫**] 工作區中，按一下頂端 [首頁] 索引標籤上的 [**建立 VM 範本**]。

2. 在 [選取來源]**** 頁面上，按一下 [使用現有的 VM 範本或存放於程式庫的虛擬硬碟]****，然後按一下 [瀏覽]****。

3. 在出現的視窗中，從 VMM 程式庫選取備妥的範本磁片。 若要更輕鬆地識別準備好哪些磁片，請以滑鼠右鍵按一下資料行標頭，並啟用**受防護**資料行。 按 **[下一步** **]** 。

4. 指定 VM 範本名稱和選擇性的描述，然後按 **[下一步]**。

5. 在 [**設定硬體**] 頁面上，指定從這個範本建立之 vm 的功能。 請確定至少有一個 NIC 可供使用，並已在 VM 範本上設定。 租使用者連線到受防護 VM 的唯一方式是透過遠端桌面連線、Windows 遠端管理或其他預先設定的遠端系統管理工具，在網路通訊協定上工作。

    如果您選擇利用 VMM 中的靜態 IP 集區，而不是在租使用者網路上執行 DHCP 伺服器，您必須向您的租使用者發出此設定的警示。 當租使用者提供其防護資料檔案（其中包含 VMM 的自動安裝檔案）時，必須為靜態 IP 集區資訊提供特殊的預留位置值。 如需租使用者自動安裝檔案中 VMM 預留位置的詳細資訊，請參閱[建立回應](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file)檔案。

6. 在 [**設定作業系統**] 頁面上，VMM 只會顯示受防護 vm 的幾個選項，包括產品金鑰、時區和電腦名稱稱。 租使用者會透過防護資料檔案（）指定某些安全資訊（例如系統管理員密碼和功能變數名稱）。PDK 檔案）。

    > [!NOTE]
    > 如果您選擇在此頁面上指定產品金鑰，請確定它適用于範本磁片上的作業系統。 如果使用了不正確的產品金鑰，則建立 VM 將會失敗。

建立範本之後，租使用者可以使用它來建立新的虛擬機器。 您必須確認 VM 範本是租用戶系統管理員消費者角色可用的資源之一（在 VMM 中，使用者角色位於 [**設定**] 工作區）。

## <a name="prepare-and-protect-the-vhdx-using-powershell"></a>使用 PowerShell 來準備和保護 VHDX

除了執行「範本磁片」 Wizard 以外，您還可以將您的範本磁片和憑證複製到執行 RSAT 的電腦，並執行 TemplateDisk 來起始簽署[程式](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps
)。
下列範例會使用_TemplateName_和_version_參數所指定的名稱和版本資訊。
您提供給參數的 VHDX `-Path` 會被更新的範本磁片覆寫，因此請務必在執行命令之前複製複本。

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Certificate $certificate -Path "WindowsServer2019-ShieldedTemplate.vhdx" -TemplateName "Windows Server 2019" -Version 1.0.0.0
```

您的範本磁片現在已準備好用來布建受防護的 Vm。
如果您使用 System Center Virtual Machine Manager 來部署 VM，您現在可以將 VHDX 複製到 VMM 程式庫。

您也可能想要從 VHDX 解壓縮磁片區簽章類別目錄。
此檔案是用來提供有關簽署憑證、磁片名稱和版本的資訊給想要使用範本的 VM 擁有者。
他們需要將此檔案匯入到 [防護資料檔案] 中，以授權您（擁有簽署憑證的範本作者）為其建立此和未來的範本磁片。

若要將磁片區簽章目錄解壓縮，請在 PowerShell 中執行下列命令：

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```


## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [建立防護資料檔案](guarded-fabric-tenant-creates-shielding-data.md)

## <a name="additional-references"></a>其他參考

- [適用於受防護主機和受防護 VM 的託管服務提供者設定步驟](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)

---
title: 建立 Windows 受防護的 VM 範本磁片
description: 深入瞭解：建立 Windows 受防護的 VM 範本磁片
ms.topic: article
ms.assetid: 9c8b84e8-1f5a-47a1-83ca-b1dbd801cb0b
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 01/29/2019
ms.openlocfilehash: 4a8d8429373e6ebedc1b6d9b828527babeb98f0e
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049806"
---
# <a name="create-a-windows-shielded-vm-template-disk"></a>建立 Windows 受防護的 VM 範本磁片

>適用于： Windows Server (半年通道) 、Windows Server 2016、Windows Server 2019


如同一般 Vm，您可以建立 VM 範本 (例如 [Virtual Machine Manager (VMM 中的 vm 範本) ](/system-center/vmm/library-vm-templates)) ，讓租使用者和系統管理員輕鬆地使用範本磁片在網狀架構上部署新的 vm。 由於受防護的 Vm 是安全性敏感的資產，因此需要額外的步驟來建立支援防護的 VM 範本。 本主題涵蓋在 VMM 中建立受防護的範本磁片和 VM 範本的步驟。

若要瞭解本主題如何納入部署受防護 Vm 的整體程式，請參閱 [主機服務提供者設定步驟，以瞭解受防護主機和受防護的 vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="prepare-an-operating-system-vhdx"></a>準備作業系統 VHDX

先準備 OS 磁片，然後再透過受防護的範本磁片建立嚮導來執行。 此磁片將用作租使用者 Vm 中的 OS 磁片。 您可以使用任何現有的工具來建立此磁片，例如 Microsoft 桌面映射 Service Manager (DISM) ，或手動設定具有空白 VHDX 的 VM，並將作業系統安裝到該磁片。 設定磁片時，必須遵守第2代和/或受防護 Vm 的特定需求：

| VHDX 的需求 | 原因 |
|-----------|----|
|必須是 (GPT) 磁片的 GUID 磁碟分割表格 | 第2代虛擬機器需要支援 UEFI|
|磁片類型必須是「 **基本** 」，而非「 **動態**」。 <br>注意：這是指邏輯磁片類型，而不是 Hyper-v 支援的「動態擴充」 VHDX 功能。 | BitLocker 不支援動態磁碟。|
|磁片至少有兩個磁碟分割。 一個磁碟分割必須包含安裝 Windows 的磁片磁碟機。 此為 BitLocker 將會加密的磁碟機。 另一個磁碟分割是使用中的磁碟分割，其中包含開機載入器並保持未加密狀態，讓電腦可以啟動。|BitLocker 所需|
|檔案系統是 NTFS | BitLocker 所需|
|VHDX 上安裝的作業系統是下列其中一項：<br>-Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 <br>-Windows 10、Windows 8.1、Windows 8| 支援第2代虛擬機器和 Microsoft 安全開機範本所需|
|作業系統必須一般化 (執行 sysprep.exe)  | 範本布建牽涉到專用 Vm 的特定租使用者工作負載|

> [!NOTE]
> 如果您使用 VMM，請勿在此階段將範本磁碟複製到 VMM 程式庫。

## <a name="run-windows-update-on-the-template-operating-system"></a>在範本作業系統上執行 Windows Update

在範本磁片上，確認作業系統已安裝所有最新的 Windows 更新。 最近發行的更新改善了端對端防護程式的可靠性，也就是如果範本作業系統不是最新版本，可能無法完成的進程。

## <a name="prepare-and-protect-the-vhdx-with-the-template-disk-wizard"></a>使用範本磁片 wizard 準備和保護 VHDX

若要使用包含受防護 Vm 的範本磁片，您必須使用受防護的範本磁片建立嚮導，以 BitLocker 準備和加密磁片。 此 wizard 將會產生磁片的雜湊，並將其新增至磁片區簽章目錄， (VSC) 。 VSC 會使用您指定的憑證進行簽署，並在布建程式期間使用，以確保為租使用者所部署的磁片未被變更或取代為租使用者不信任的磁片。 最後，BitLocker 會安裝在磁片的作業系統 (如果) 尚未安裝，請在 VM 布建期間將磁片準備好進行加密。

> [!NOTE]
> 範本磁片嚮導將會修改您就地指定的範本磁片。 您可能會想要在執行嚮導之後，先複製未受保護的 VHDX，稍後再進行磁片更新。 您將無法修改使用範本磁片嚮導保護的磁片。

在執行 Windows Server 2016 的電腦上執行下列步驟，Windows 10 (使用遠端伺服器管理工具、安裝的 RSAT) 或更新版本 (不需要是受防護的主機或 VMM 伺服器) ：

1. 將在「 [準備作業系統 VHDX](#prepare-an-operating-system-vhdx) 」中建立的一般化 VHDX 複製到伺服器（如果尚未存在）。

2. 若要在本機管理伺服器，請從伺服器上的 **遠端伺服器管理工具** 安裝 **受防護的 VM 工具** 功能。

    ```
    Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
    ```

    您也可以從已安裝 [Windows 10 遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)的用戶端電腦來管理伺服器。

3. 取得或建立憑證來簽署 VHDX 的 VSC，以成為新受防護 Vm 的範本磁片。 當租使用者建立防護資料檔案並授權其信任的磁片時，將會向租使用者顯示此憑證的詳細資料。 因此，請務必從您和您的租使用者所信任的憑證授權單位單位取得此憑證。 在您同時為主機和租使用者的企業案例中，您可以考慮從 PKI 發行此憑證。

    如果您要設定測試環境，而只想要使用自我簽署憑證來準備您的範本磁片，請執行類似下列的命令：

    ```
    New-SelfSignedCertificate -DnsName publisher.fabrikam.com
    ```

4. 從 [開始] 功能表上的 [系統 **管理工具**] 資料夾，或在命令提示字元中輸入 **TemplateDiskWizard.exe** 來啟動 **範本磁片 Wizard** 。

5. 在 [ **憑證** ] 頁面上，按一下 **[流覽]** 以顯示憑證清單。 選取用來準備磁片範本的憑證。 按一下 **[確定]**，然後按 **[下一步]**。

6. 在 [虛擬磁片] 頁面上，按一下 **[流覽]** 以選取您已備妥的 VHDX，然後按 **[下一步]**。

7. 在 [簽章目錄] 頁面上，提供易記的 **磁片名稱** 和 **版本。** 這些欄位可協助您在備妥磁片之後加以識別。

    例如，您可以針對 **磁片名稱** 輸入 _WS2016_ 和 _1.0.0.0_ **版**

8. 在嚮導的 [審核設定] 頁面上，檢查您的選擇。 當您按一下 [ **產生**] 時，嚮導會在範本磁片上啟用 BitLocker、計算磁片的雜湊，並建立儲存在 VHDX 中繼資料中的磁片區簽章目錄。

    等到準備程式完成，再嘗試掛接或移動範本磁片。 此程式可能需要一段時間才能完成，視您磁片的大小而定。

    > [!IMPORTANT]
    > 範本磁片只能搭配安全受防護的 VM 布建程式使用。
    > 使用範本磁片嘗試開機一般 (非遮罩) VM，可能會在) 畫面且不受支援 (時產生停止錯誤。

9. 在 [ **摘要** ] 頁面上，會顯示磁片範本的相關資訊、用來簽署 VSC 的憑證，以及憑證簽發者。 按一下 [關閉] 結束精靈。

如果您使用 VMM，請依照本主題其餘章節中的步驟，將範本磁片併入 VMM 中受防護的 VM 範本。

## <a name="copy-the-template-disk-to-the-vmm-library"></a>將範本磁碟複製到 VMM 程式庫

如果您使用 VMM，在建立範本磁片之後，您必須將它複製到 VMM 程式庫共用，讓主機可以在布建新的 Vm 時下載並使用磁片。 使用下列程式，將範本磁碟複製到 VMM 程式庫，然後重新整理程式庫。

1. 將 VHDX 檔案複製到 VMM 程式庫共用資料夾。 如果您使用預設的 VMM 設定，請將範本磁碟複製到 _\\ <vmmserver> \MSSCVMMLibrary\VHDs_。

2. 重新整理程式庫伺服器。 開啟 [連結 **庫** ] 工作區，展開 [連結 **庫伺服器**]，在您要重新整理的程式庫伺服器 **上按一下滑鼠** 右鍵，然後按一下 [重新整理]。

3. 接下來，為 VMM 提供安裝在範本磁片上之作業系統的相關資訊：

    a. 在 [連結 **庫** ] 工作區中的程式庫伺服器上，尋找新匯入的範本磁片。

    b. 在磁片 **上按一下滑鼠** 右鍵，然後按一下 [內容]。

    c. 針對 [ **作業系統**]，展開清單，然後選取安裝在磁片上的作業系統。 選取作業系統會向 VMM 指出 VHDX 不是空白的。

    d. 更新內容之後，按一下 [確定]。

磁片名稱旁邊的小盾牌圖示代表磁片是受防護 Vm 的備妥範本磁片。 您也可以用滑鼠右鍵按一下資料行標頭，並切換 **受防護** 的資料行，以查看指出磁片是否適用于一般或受防護 VM 部署的文字標記法。

![受防護的 vm 範本磁片](../media/Guarded-Fabric-Shielded-VM/shielded-vm-template-disk.png)

## <a name="create-the-shielded-vm-template-in-vmm-using-the-prepared-template-disk"></a>使用備妥的範本磁片在 VMM 中建立受防護的 VM 範本

在 VMM 程式庫中備妥範本磁片之後，您就可以開始為受防護的 Vm 建立 VM 範本。 受防護 Vm 的 VM 範本與傳統的 VM 範本稍有不同，) 因為某些設定已修正 (第2代 VM、已啟用 UEFI 和安全開機等，而其他設定則無法使用 (租使用者自訂僅限於少數，請選取 VM) 的屬性。 若要建立 VM 範本，請執行下列步驟：

1. 在 [連結 **庫** ] 工作區的頂端，按一下 [首頁] 索引標籤上的 [ **建立 VM 範本** ]。

2. 在 [選取來源] 頁面上，按一下 [使用現有的 VM 範本或存放於程式庫的虛擬硬碟]，然後按一下 [瀏覽]。

3. 在出現的視窗中，選取 VMM 程式庫中已備妥的範本磁片。 若要更輕鬆地識別準備的磁片，請以滑鼠右鍵按一下資料行標頭，並啟用 **受防護** 的資料行。 按一下 **[確定** **]，然後** 按一下 [確定]。

4. 指定 VM 範本名稱並選擇性地指定描述，然後按 **[下一步]**。

5. 在 [ **設定硬體** ] 頁面上，指定從這個範本建立之 vm 的功能。 請確定至少有一個 NIC 可供使用，且已在 VM 範本上設定。 租使用者連線到受防護 VM 的唯一方法是透過遠端桌面連線、Windows 遠端管理或其他預先設定的遠端系統管理工具，這些工具可透過網路通訊協定運作。

    如果您在 VMM 中選擇利用靜態 IP 集區，而不是在租使用者網路上執行 DHCP 伺服器，則您必須對租使用者發出這項設定的警示。 當租使用者提供其防護資料檔案（其中包含 VMM 的自動安裝檔案）時，必須為靜態 IP 集區資訊提供特殊的預留位置值。 如需租使用者自動安裝檔案中 VMM 預留位置的詳細資訊，請參閱 [建立回應](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file)檔案。

6. 在 [ **設定作業系統** ] 頁面上，VMM 只會顯示受防護 vm 的幾個選項，包括產品金鑰、時區和電腦名稱稱。 某些安全資訊（例如系統管理員密碼和功能變數名稱）是由租使用者透過防護資料檔案 ( 所指定。PDK 檔案) 。

    > [!NOTE]
    > 如果您選擇在此頁面上指定產品金鑰，請確定它適用于範本磁片上的作業系統。 如果使用不正確的產品金鑰，VM 建立將會失敗。

建立範本之後，租使用者就可以使用它來建立新的虛擬機器。 您必須確認 VM 範本是 VMM 中租用戶系統管理員消費者角色 (可用的其中一個資源，使用者角色位於 [ **設定** ] 工作區) 。

## <a name="prepare-and-protect-the-vhdx-using-powershell"></a>使用 PowerShell 準備和保護 VHDX

除了執行範本磁片嚮導之外，您還可以將您的範本磁片和憑證複製到執行 RSAT 的電腦，然後執行 [TemplateDisk](/powershell/module/shieldedvmtemplate/protect-templatedisk) 來起始簽署程式。
下列範例會使用 _TemplateName_ 和 _version_ 參數所指定的名稱和版本資訊。
您提供給參數的 VHDX `-Path` 將會以更新的範本磁片覆寫，因此請務必在執行命令之前建立複本。

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Certificate $certificate -Path "WindowsServer2019-ShieldedTemplate.vhdx" -TemplateName "Windows Server 2019" -Version 1.0.0.0
```

您的範本磁片現在已準備好用來布建受防護的 Vm。
如果您使用 System Center Virtual Machine Manager 部署 VM，您現在可以將 VHDX 複製到 VMM 程式庫。

您也可能想要從 VHDX 解壓縮磁片區簽章目錄。
此檔案是用來將簽署憑證、磁片名稱和版本的相關資訊提供給想要使用範本的 VM 擁有者。
他們必須將此檔案匯入防護資料檔案嚮導，以授權您（擁有簽署憑證的範本作者）為其建立此和未來的範本磁片。

若要解壓縮磁片區簽章目錄，請在 PowerShell 中執行下列命令：

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```


## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [建立防護資料檔案](guarded-fabric-tenant-creates-shielding-data.md)

## <a name="additional-references"></a>其他參考資料

- [適用於受防護主機和受防護 VM 的託管服務提供者設定步驟](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)

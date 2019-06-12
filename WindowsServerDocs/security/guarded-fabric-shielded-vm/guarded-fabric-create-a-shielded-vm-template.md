---
title: 建立 Windows 受防護的 VM 範本磁碟
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 9c8b84e8-1f5a-47a1-83ca-b1dbd801cb0b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/29/2019
ms.openlocfilehash: d9f07d2e6e93d4f8d198c2fc3b62c28c940bdefb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447516"
---
# <a name="create-a-windows-shielded-vm-template-disk"></a>建立 Windows 受防護的 VM 範本磁碟

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

由於使用一般的 Vm，您可以建立 VM 範本 (例如[Virtual Machine Manager (VMM) 中的 VM 範本](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-library-add-vm-templates)) 讓您輕鬆部署新的 Vm 使用的範本磁碟在網狀架構上的租用戶和系統管理員。 受防護的 Vm 是安全性敏感的資產，因為有額外的步驟來建立已支援防護的 VM 範本。 本主題涵蓋如何在 VMM 中建立受防護的範本磁碟和 VM 範本。

若要了解本主題如何放入部署受防護的 Vm 的整體程序，請參閱[裝載服務提供者設定步驟，針對受防護主機和受防護的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="prepare-an-operating-system-vhdx"></a>準備作業系統 VHDX

首先準備的 OS 磁碟，您便會執行經由受防護的範本磁碟建立精靈。 此磁碟將用於您的租用戶 Vm 的 OS 磁碟。 您可以使用任何現有的工具來建立此磁碟，例如 Microsoft 桌面映像服務管理員 」 (DISM)，或以手動方式設定具有空白的 VHDX 的 VM 並安裝到該磁碟上的作業系統。 當設定磁碟，它必須遵守下列需求，專屬於第 2 代及/或受防護的 Vm: 

| Vhdx 的需求 | `Reason` |
|-----------|----|
|必須是 GUID 磁碟分割表格 (GPT) 磁碟 | 所需的第 2 代虛擬機器，才能支援 UEFI|
|磁碟類型必須是**基本**相對於**動態**。 <br>注意:這是指邏輯磁碟類型，而不"動態擴充的 」 VHDX 支援之功能的 HYPER-V。 | BitLocker 不支援動態磁碟。|
|磁碟會有至少兩個資料分割。 一個磁碟分割必須包含 Windows 安裝所在的磁碟機。 這是 BitLocker 將要加密的磁碟機。 另一個磁碟分割為作用中的磁碟分割，其中包含開機載入器並不會加密，以便可以啟動電腦。|所需的 BitLocker|
|檔案系統是 NTFS | 所需的 BitLocker|
|安裝在 VHDX 上的作業系統可以是下列其中一項：<br>Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 <br>-Windows 10，Windows 8.1，Windows 8| 需要支援第 2 代虛擬機器和 Microsoft 安全開機範本|
|作業系統必須一般化 (執行的 sysprep.exe) | 範本佈建包含專門針對特定租用戶的工作負載的 Vm| 

> [!NOTE]
> 如果您使用 VMM，請勿複製範本磁碟至 VMM 程式庫在這個階段。 

## <a name="run-windows-update-on-the-template-operating-system"></a>範本作業系統上執行 Windows Update

範本磁碟上，確認作業系統具有所有最新安裝的 Windows 更新。 最近發行的更新，改善端對端防護程序的可靠性-可能無法完成如果範本作業系統的程序不是最新狀態。

## <a name="prepare-and-protect-the-vhdx-with-the-template-disk-wizard"></a>準備並保護範本磁碟精靈 」 使用 VHDX

若要使用受防護的 Vm 範本磁碟，必須備妥並使用受防護的範本磁碟建立精靈中使用 BitLocker 加密磁碟。 此精靈會產生磁碟的雜湊值，並將它新增至磁碟區簽章目錄 (VSC)。 VSC 使用您指定，並在佈建程序期間用來確保要部署的租用戶之磁碟不已改變或取代的磁碟，租用戶不信任的憑證來簽署。 最後，BitLocker 安裝磁碟的作業系統上 （如果它已經不存在） 來準備 VM 佈建期間加密的磁碟。

> [!NOTE]
> 範本磁碟精靈 將會修改您指定位置中的範本磁碟。 若要再執行對磁碟進行更新，稍後在精靈中建立一份未受保護的 VHDX。 您無法修改範本磁碟精靈 與受保護的磁碟。

執行 Windows Server 2016 （不需要是受守護的主機或 VMM 伺服器） 的電腦上執行下列步驟：

1. 複製在中建立一般化的 VHDX[準備作業系統 VHDX](#prepare-an-operating-system-vhdx)到伺服器，如果它已經不存在。

2. 若要在本機管理伺服器，安裝**受防護的 VM 工具**功能**遠端伺服器管理工具**伺服器上。

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
        
    您也可以管理您已安裝的用戶端電腦的伺服器[Windows 10 遠端伺服器管理工具](https://www.microsoft.com/en-us/download/details.aspx?id=45520)。

3. 取得或建立憑證來簽署 VSC 會變成新的受防護 vm 的範本磁碟的 vhdx。 當他們建立其虛擬機器防護資料檔案，並在已授權他們信任的磁碟時，此憑證的詳細資訊將會顯示給租用戶。 因此，務必從相互信任您和您的租用戶之憑證授權單位取得此憑證。 在您所在的主機服務提供者和租用戶的企業案例，您可以考慮從您的 PKI 發行此憑證。

    如果您要設定測試環境，而只想要使用自我簽署的憑證來準備您的範本磁碟，執行的命令如下所示：

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. 開始**範本磁碟精靈**從**系統管理工具**資料夾，在 [開始] 功能表上，或輸入**TemplateDiskWizard.exe**在命令提示字元。

5. 在 **憑證**頁面上，按一下**瀏覽**顯示一份憑證。 選取用來準備磁碟範本的憑證。 按一下 **[確定]** ，然後按 **[下一步]** 。

6. 在虛擬磁碟 頁面上，按一下**瀏覽**，選取您已備妥的 VHDX，然後按一下**下一步**。

7. 在簽章目錄] 頁面上，提供 [易記**磁碟名稱**和**版本。** 這些欄位會出現以協助您識別磁碟，一旦已備妥。

    例如，對於**磁碟名稱**您可以輸入_WS2016_至於**版本**， _1.0.0.0_

8. 檢閱您在精靈的 [檢閱設定] 頁面的選項。 當您按一下 **產生**，精靈會的範本磁碟上啟用 BitLocker、 計算的雜湊的磁碟，並建立磁碟區簽章目錄，其儲存於 VHDX 中繼資料。

    等到準備程序完成之前嘗試掛接或移動的範本磁碟。 此程序可能需要一段時間才能完成，視您的磁碟大小而定。

    > [!IMPORTANT]
    > 範本磁碟只可以搭配安全的受防護 VM 佈建程序。
    > 嘗試啟動一般 （受防護） 使用的範本磁碟的 VM 將會停止錯誤 （藍色螢幕） 的可能結果，並不支援。

9. 在 **摘要**頁面上，磁碟 範本，用來簽署 VSC，憑證的資訊，而且會顯示憑證簽發者。 按一下 **[關閉]** 以結束精靈。

如果您使用 VMM，請遵循本主題，以納入受防護的 VM 範本，在 VMM 中的範本磁碟的其餘章節中的步驟。 

## <a name="copy-the-template-disk-to-the-vmm-library"></a>將範本磁碟複製到 VMM 程式庫

如果您建立的範本磁碟之後，您可以使用 VMM 中，您需要將它複製到 VMM 程式庫共用，讓主機可以下載並使用該磁碟佈建的新 Vm 時。 您可以使用下列程序來複製到 VMM 程式庫的範本磁碟，然後重新整理程式庫。

1. 將 VHDX 檔案複製到 VMM 程式庫共用資料夾中。 如果您使用預設的 VMM 組態時，將複製的範本磁碟 _\\ <vmmserver>\MSSCVMMLibrary\VHDs_。

2. 重新整理程式庫伺服器。 開啟**程式庫**工作區中，展開**程式庫伺服器**，以滑鼠右鍵按一下您想要重新整理，請在程式庫伺服器上，按一下 **重新整理**。

3. 接下來，VMM 提供的範本磁碟上安裝作業系統的相關資訊：

    a. 在您的程式庫伺服器上尋找您剛匯入的範本磁碟**程式庫**工作區。

    b. 以滑鼠右鍵按一下磁碟，然後按一下**屬性**。

    c. 針對**作業系統**，展開清單並選取安裝在磁碟上的作業系統。 選取作業系統會向 VMM 指出 VHDX 不是空白。

    d. 更新內容之後，按一下 [確定]  。

磁碟名稱旁邊的小盾牌圖示會表示為已備妥的範本磁碟的磁碟，受防護的 vm。 您也可以以滑鼠右鍵按一下資料行標頭和切換**防護**看到指出磁碟是否適用於一般或受防護的 VM 部署的文字表示的資料行。

![受防護的 vm 範本磁碟](../media/Guarded-Fabric-Shielded-VM/shielded-vm-template-disk.png)

## <a name="create-the-shielded-vm-template-in-vmm-using-the-prepared-template-disk"></a>使用已備妥的範本磁碟在 VMM 中建立受防護的 VM 範本

使用您的 VMM 程式庫中備妥的範本磁碟，您就能夠為受防護的 Vm 建立 VM 範本。 受防護的 Vm 的 VM 範本稍微不同傳統 VM 範本，因為某些設定固定的 （2 個 VM、 UEFI 和安全開機已啟用，產生等等），有些則是無法使用 （租用戶自訂僅限於少數，選取 [屬性] 的 VM）. 若要建立 VM 範本，請執行下列步驟：

1. 在 **程式庫**工作區中，按一下**建立 VM 範本**頂端的 首頁 索引標籤。

2. 在 [選取來源]  頁面上，按一下 [使用現有的 VM 範本或存放於程式庫的虛擬硬碟]  ，然後按一下 [瀏覽]  。

3. 在出現的視窗中，選取已備妥的範本磁碟從 VMM 程式庫。 若要更輕鬆地識別哪些磁碟已準備，以滑鼠右鍵按一下資料行標頭，並啟用**防護**資料行。 按一下 [ **[確定]** 再**下一步]** 。

4. 指定 VM 範本名稱和選擇性描述，然後再按一下**下一步**。

5. 在 **設定硬體**頁面上，指定從這個範本建立 Vm 的功能。 請確定至少一個 NIC 可用，且設定在 VM 範本。 對連接到受防護 VM 的租用戶的唯一方式是透過遠端桌面連線、 Windows 遠端管理或其他預先設定的遠端管理工具可透過網路通訊協定。

    如果您選擇使用在 VMM 中的靜態 IP 集區，而不是執行 DHCP 伺服器租用戶網路上，您必須提醒您的租用戶，此設定。 當租用戶提供其防護資料檔案，其中包含 VMM 自動安裝檔案，他們必須提供靜態 IP 集區資訊的特殊預留位置值。 如需租用戶自動安裝檔案中的 VMM 預留位置的詳細資訊，請參閱[建立回應檔案](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file)。 

6. 在 [ **Configure Operating System** ] 頁面上，VMM 只會顯示幾個選項，針對受防護的 Vm，包括產品金鑰、 時區和電腦名稱。 有些安全的資訊，例如系統管理員密碼和網域名稱，透過防護資料檔案時由租用戶 (。PDK 檔案）。 

    > [!NOTE]
    > 如果您選擇在此頁面指定產品金鑰，請確定它是適用於範本磁碟上的作業系統。 如果使用不正確的產品金鑰時，將無法建立 VM。

建立範本之後，租用戶可以使用它來建立新的虛擬機器。 您必須確認 VM 範本是其中一個租用戶系統管理員使用者角色的可用資源 (在 VMM 中，使用者角色都處於**設定**工作區)。

## <a name="prepare-and-protect-the-vhdx-using-powershell"></a>準備並保護使用 PowerShell 的 VHDX

為執行範本磁碟精靈 的替代方案，您可以將您的範本磁碟和憑證複製到執行 RSAT 的電腦，並執行[保護 TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps
)起始簽署程序。
下列範例會使用所指定的名稱和版本資訊_TemplateName_並_版本_參數。
您提供給 VHDX`-Path`參數將會覆寫與更新後的範本磁碟，因此請務必執行命令之前製作的複本。

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Certificate $certificate -Path "WindowsServer2019-ShieldedTemplate.vhdx" -TemplateName "Windows Server 2019" -Version 1.0.0.0
```

現在準備好要用來佈建受防護 Vm 的範本磁碟。
如果您使用 System Center Virtual Machine Manager 來部署 VM，您現在可以將 VHDX 複製到 VMM 程式庫。

您也可以從 VHDX 擷取磁碟區簽章目錄。
這個檔案用來簽署憑證、 磁碟名稱和版本資訊提供 VM 擁有者想要使用您的範本。
他們必須將此檔案匯入授權您擁有的簽章憑證，建立此範本作者防護資料檔案精靈 和未來的範本磁碟它們。

若要擷取的磁碟區簽章目錄，請在 PowerShell 中執行下列命令：

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```


## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [建立虛擬機器防護資料檔案](guarded-fabric-tenant-creates-shielding-data.md)

## <a name="see-also"></a>另請參閱

- [裝載服務提供者設定步驟，針對受防護主機和受防護的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)

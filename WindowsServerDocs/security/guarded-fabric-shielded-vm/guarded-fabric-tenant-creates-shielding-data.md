---
title: 受防護的 Vm，可供租用戶-若要定義受防護的 VM 建立虛擬機器防護資料
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 49f4e84d-c1f7-45e5-9143-e7ebbb2ef052
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: 1245b88a42b80218b5557dc89f2b97b5d0059d44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852539"
---
# <a name="shielded-vms-for-tenants---creating-shielding-data-to-define-a-shielded-vm"></a>受防護的 Vm，可供租用戶-若要定義受防護的 VM 建立虛擬機器防護資料

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

防護資料檔案 (也稱為佈建資料檔案或 PDK 檔案) 是一種加密檔案，租用戶或 VM 擁有者會建立該檔案以保護重要的 VM 組態資訊，例如系統管理員密碼、RDP 及其他身分識別相關的憑證、網域加入認證等等。 本主題提供如何建立防護資料檔案的相關資訊。 您可以建立檔案之前，您必須從裝載的服務提供者取得的範本磁碟，或建立範本磁碟，如中所述[受防護的 Vm，可供租用戶-建立範本磁碟 （選擇性）](guarded-fabric-tenant-creates-template-disk.md)。

如需清單和圖表的防護資料檔案的內容，請參閱 <<c0> [ 什麼防護資料，以及它為何如此必要？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)。

> [!IMPORTANT]
> 在本節中的步驟應該執行 Windows Server 2016 的租用戶電腦上完成。 該電腦必須不是受防護網狀架構的一部分 （也就是不應設定為使用 HGS 叢集）。

若要準備建立防護資料檔案，請採取下列步驟：

- [取得遠端桌面連線的憑證](#obtain-a-certificate-for-remote-desktop-connection)
- [建立回應檔案](#create-an-answer-file)
- [取得磁碟區簽章目錄檔案](#get-the-volume-signature-catalog-file)
- [選取 受信任的網狀架構](#select-trusted-fabrics)

然後您可以建立防護資料檔案：

- [建立虛擬機器防護資料檔案，並新增守護者](#create-a-shielding-data-file-and-add-guardians)


## <a name="obtain-a-certificate-for-remote-desktop-connection"></a>取得遠端桌面連線的憑證

由於租用戶都只是能夠連線到其受防護的 Vm，使用遠端桌面連線或其他遠端管理工具時，務必確保租用戶可以確認他們連接到正確的端點 （也就是沒有 「 攔截式 」攔截連線）。

請確認您已連接到想要的伺服器的一種方法是安裝和設定來呈現您初始連線時的遠端桌面服務的憑證。 連接到伺服器的用戶端電腦會檢查是否信任的憑證和顯示一則警告則。 一般而言，若要確保連線的用戶端信任的憑證，RDP 憑證發行從租用戶的 PKI。 詳細資訊[遠端桌面服務使用憑證](https://technet.microsoft.com/library/dn781533.aspx)可以在 TechNet 上找到。

<!-- The previous link comes from Windows 2012 R2 content, but as of Sept 2016, there isn't a more recent link that covers the same information. -->

> [!NOTE]
> 選取要包含在您的防護資料檔案中的 RDP 憑證，請務必使用萬用字元憑證。 可用來建立無限的數量的 Vm 設定了一個虛擬機器防護資料檔案。 因為每個 VM 會共用相同的憑證，萬用字元憑證的憑證可確保不論 VM 的主機名稱有效。

如果您正在評估受防護的 Vm，但卻尚未準備好從您的憑證授權單位要求憑證，您可以建立自我簽署的憑證在租用戶電腦上執行下列 Windows PowerShell 命令 (其中*contoso.com*是租用戶的網域):

``` powershell
$rdpCertificate = New-SelfSignedCertificate -DnsName '\*.contoso.com'
$password = ConvertTo-SecureString -AsPlainText 'Password1' -Force
Export-PfxCertificate -Cert $RdpCertificate -FilePath .\rdpCert.pfx -Password $password
```

## <a name="create-an-answer-file"></a>建立回應檔案

在 VMM 中的已簽署的範本磁碟已一般化，因為租用戶，才能提供回應檔案以在佈建程序期間是專為受防護的 Vm。 回應檔案 （通常稱為自動安裝檔案） 可以設定其預期的角色的 VM-也就是它可以安裝 Windows 功能、 註冊 RDP 憑證在上一個步驟中，建立和執行其他自訂動作。 它也會提供 Windows 安裝程式，包括預設系統管理員的密碼和產品金鑰所需的資訊。

如需有關如何取得及使用資訊**新增 ShieldingDataAnswerFile**函式來產生回應檔案 （Unattend.xml 檔案） 來建立受防護的 Vm，請參閱[利用產生的回應檔案新 ShieldingDataAnswerFile 函式](guarded-fabric-sample-unattend-xml-file.md)。 使用此函式，您可以更輕鬆地產生回應檔案，以反映選項，如下所示：

- 就是要在初始化程序結尾已加入網域的 VM？
- 將您使用大量授權或每個 VM 特定的產品金鑰？
- 您使用 DHCP 或靜態 IP？
- 您將使用遠端桌面通訊協定 (RDP) 憑證將用以證明 VM 隸屬於您的組織？
- 執行指令碼在初始化結尾嗎？
- 您會有進一步的組態使用 Desired State Configuration (DSC) 伺服器？

防護資料檔案所用的回應檔案將用於建立使用該虛擬機器防護資料檔案的每個 VM。 因此，您應該要確定，您執行沒有硬式編碼到回應檔案的任何特定 VM 的資訊。 VMM 支援 （請參閱下表） 的一些替代字串，以處理可能會因 VM 至 VM 的特製化值的自動安裝檔案中。 您不需要使用這些項目;不過，如果有 VMM 會利用它們。

當受防護的 vm 建立 unattend.xml 檔案，請記住下列限制：

-   正在關閉之後它已設定, 的 VM 必須造成自動安裝檔案。 這是為了讓 VMM 知道當它應該報告為租用戶 VM 完成佈建，並可供使用。 一旦系統偵測到它已在佈建期間已關閉 VMM 自動會重新開啟電源的 VM。

-   強烈建議您設定以確保您要連接到 VM 的權限和攔截攻擊所設定的不另一部機器的 RDP 憑證。

-   請務必啟用 RDP 和對應的防火牆規則，以便設定它之後，您可以存取 VM。 因此，您必須連接到您的 VM 的 RDP，您無法使用 VMM 主控台，以存取受防護的 Vm。 如果您想要管理您的系統，使用 Windows PowerShell 遠端執行功能，請確定已啟用 WinRM，太。

-   支援受防護的 VM 自動安裝檔案的唯一的替代字串如下所示：

| 可取代的項目 | 替代字串 |
|-----------|-----------|
| ComputerName        | @ComputerName@      |
| TimeZone            | @TimeZone@          |
| ProductKey          | @ProductKey@        |
| IPAddr4-1           | @IP4Addr-1@         |
| IPAddr6-1           | @IP6Addr-1@         |
| MACAddr-1           | @MACAddr-1@         |
| Prefix-1-1          | @Prefix-1-1@        |
| NextHop-1-1         | @NextHop-1-1@       |
| Prefix-1-2          | @Prefix-1-2@        |
| NextHop-1-2         | @NextHop-1-2@       |

使用替代字串時，務必確保在 VM 佈建程序期間，將會填入的字串。 如果這種字串@ProductKey@ 未提供在部署階段，離開&lt;ProductKey&gt;空白的自動安裝檔案中的節點，特製化程序將會失敗，您將無法連接到您的 VM。

另請注意是否確實 VMM 靜態 IP 位址集區，則只會使用資料表的結尾網路相關功能的替代字串。 您的主機服務提供者應該能夠告訴您是否需要這些替代字串。 如需有關 VMM 範本中的靜態 IP 位址的詳細資訊，請參閱 VMM 文件中的下列：

- [IP 位址集區的指導方針](https://technet.microsoft.com/system-center-docs/vmm/plan/plan-network#guidelines-for-ip-address-pools) 
- [設定 VMM 光纖中的靜態 IP 位址集區](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-network-static-address-pools)

最後，務必要注意的受防護的 VM 部署程序只會加密 OS 磁碟機。 如果您部署受防護的 VM 與一或多個資料磁碟機，強烈建議您新增 unattend 命令或群組原則設定，在租用戶網域來自動加密資料磁碟機。

## <a name="get-the-volume-signature-catalog-file"></a>取得磁碟區簽章目錄檔案

防護資料檔案也會包含租用戶信任範本磁碟有關的資訊。 租用戶取得受信任的範本磁碟的磁碟區簽章目錄 (VSC) 檔案形式從磁碟簽章。 部署新的 VM 時，會接著會驗證這些簽章。 如果沒有任何防護資料檔案中的簽章相符的範本磁碟嘗試部署的 vm （亦即它已被修改或與不同，可能是惡意的磁碟交換），佈建程序將會失敗。

> [!IMPORTANT]
> 雖然 VSC 可確保磁碟未被竄改，仍然十分重要，租用戶才能在第一時間信任磁碟。 如果您是租用戶，並由您的主機服務提供者，提供範本磁碟部署測試使用該範本磁碟的 VM，並執行您自己的工具 （例如防毒軟體、 弱點掃描器等等） 以驗證磁碟是，事實上，在您信任的狀態。

有兩種方式可取得的範本磁碟 VSC:

-  主機服務提供者 （或租用戶，如果租用戶可以存取 VMM） 來儲存 VSC 使用 VMM PowerShell cmdlet，並為其提供給租用戶。 這可以使用 VMM 主控台安裝並設定為可管理的主控網狀架構的 VMM 環境執行的任何電腦上。 若要儲存 VSC 的 PowerShell cmdlet 是：

        $disk = Get-SCVirtualHardDisk -Name "templateDisk.vhdx"
    
        $vsc = Get-SCVolumeSignatureCatalog -VirtualHardDisk $disk
    
        $vsc.WriteToFile(".\templateDisk.vsc")

-  租用戶可以存取的範本磁碟檔案。 如果租用戶建立範本磁碟上傳至託管的服務提供者，或租用戶可以下載主機服務提供者的範本磁碟，這可能是大小寫。 在此情況下，不使用 VMM 在圖片中，租用戶可以執行下列 cmdlet （與受防護的 VM 工具 」 功能，而遠端伺服器管理工具的一部分一起安裝）：

        Save-VolumeSignatureCatalog -TemplateDiskPath templateDisk.vhdx -VolumeSignatureCatalogPath templateDisk.vsc

## <a name="select-trusted-fabrics"></a>選取 受信任的網狀架構

防護資料檔案中的最後一個元件與擁有者及 VM 的守護者相關聯。 守護者用來指派這兩個受防護的 VM，並在其它已獲授權執行受防護網狀架構的擁有者。

若要授權的主控網狀架構執行受防護的 VM，您必須從主機服務提供者的主機守護者服務取得守護者中繼資料。 通常，主機服務提供者會提供您與這個中繼資料，透過其管理工具。 在企業案例中，您可能直接取得您自己的中繼資料的存取。

您的主機服務提供者可以取得守護者中繼資料從 HGS 藉由執行下列動作之一：

-  直接從 HGS 取得守護者中繼資料，請執行下列 Windows PowerShell 命令，或瀏覽至網站並儲存 XML 檔案顯示：

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

-  從 VMM 中使用 VMM PowerShell cmdlet 取得守護者中繼資料：

        $relecloudmetadata = Get-SCGuardianConfiguration

        $relecloudmetadata.InnerXml | Out-File .\RelecloudGuardian.xml -Encoding UTF8

<!-- Note that the VMM PowerShell cmdlets aren't Windows PowerShell, so "VMM PowerShell" is the correct terminology for them. -->

取得您想要授權受防護的 Vm 上執行，然後再繼續每個受防護網狀架構的守護者中繼資料檔案。

## <a name="create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard"></a>建立虛擬機器防護資料檔案，並加入使用 [防護資料檔案精靈] 的守護者

執行 [防護資料檔案精靈] 建立防護資料 (PDK) 檔案。 在這裡，您將新增 RDP 憑證、 自動安裝檔案，磁碟區簽章目錄、 擁有者守護者和下載守護者中繼資料取得上一個步驟中的功能。

1.  安裝**遠端伺服器管理工具&gt;功能管理工具&gt;受防護的 VM 工具**您使用伺服器管理員] 或 [下列 Windows PowerShell 命令的電腦上：

        Install-WindowsFeature RSAT-Shielded-VM-Tools

2.  開啟 [防護資料檔案精靈] 從 [開始] 功能表上，或藉由執行下列可執行檔中的 [系統管理工具] 區段**c:\\Windows\\System32\\ShieldingDataFileWizard.exe**。

3.  在第一個頁面上，使用第二個檔案選取方塊來選擇您的防護資料檔案的位置和檔案名稱。 一般來說，您會命名防護資料檔案使用該虛擬機器防護資料的實體擁有的任何 Vm 建立之後 (例如 HR、 IT、 財務) 並它正在執行的工作負載的角色 （如範例中，檔案伺服器、 web 伺服器，或任何其他項目設定的自動安裝檔案）。 保留設定的選項按鈕**防護資料防護範本**。

    > [!NOTE]
    > 在 [防護資料檔案精靈] 中，您會發現下列兩個選項：
    >- **防護範本的虛擬機器防護資料**
    >- **現有的 Vm 和受防護的範本的虛擬機器防護資料**<br>
    > 正在建立新的受防護 Vm，從受防護的範本時，會使用第一個選項。 第二個選項可讓您建立僅適用於當轉換現有的 Vm，或建立受防護的 Vm 從非受防護的範本的虛擬機器防護資料。

    ![防護資料檔案精靈，檔案選取](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-01.png)

       此外，您必須選擇建立的 Vm 是否使用此虛擬機器防護資料檔案將真正受防護或在 「 支援加密 」 模式中設定。 如需這兩個選項的詳細資訊，請參閱[可以執行受防護網狀架構的虛擬機器的類型是什麼？](guarded-fabric-and-shielded-vms.md#what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run)。

    > [!IMPORTANT]
    > 特別注意的下一個步驟，因為它會定義擁有者的受防護的 Vm 和受防護的 Vm 將會獲得授權，在上執行的網狀架構。<br>擁有**擁有者守護者**才能在稍後變更從現有的受防護的 VM**防護**來**支援加密**，反之亦然。
    
4.  您在此步驟中的目標是兩個方面：

    - 建立或選取代表您為 VM 擁有者擁有者守護者

    - 匯入您從主控提供者的下載守護者 （或您自己） 上一個步驟中的主機守護者服務

    若要指定現有的擁有者守護者，請從下拉式功能表中選取適當的守護者。 這份清單中，會顯示只有本機電腦上安裝具有私用的索引鍵不變的守護者。 您也可以建立您自己的擁有者守護者藉由選取**管理本機 Guardians**在右上角，然後按一下**建立**並完成精靈。

    接下來，我們匯入稍早下載守護者中繼資料使用再次**擁有者和 Guardians**頁面。 選取 **管理本機 Guardians**從右下角。 使用**匯入**守護者中繼資料檔案匯入的功能。 按一下 **確定**一旦匯入或新增所有必要的守護者。 最佳做法，請將 guardians 命名主機服務提供者或企業資料中心所代表。 最後，選取代表授權執行受防護的 VM 的資料中心的所有守護者。 您不需要再次選取擁有者守護者。 按一下 [**下一步]** 一次完成。

    ![防護資料檔案精靈 」、 「 擁有者以及 「 守護者](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-02.png)

5.  在磁碟區識別碼辨識符號 頁面上，按一下**新增**來授權虛擬機器防護資料檔案中的已簽署的範本磁碟。 當您在對話方塊中選取 VSC 時，它會顯示您該磁碟的名稱、 版本和已用來簽署憑證的相關資訊。 針對每個您想要授權的範本磁碟重複此程序。

6.  在 **特製化的值**頁面上，按一下**瀏覽**來選取將用來特製化您的 Vm 將 unattend.xml 檔案。

    使用**新增**加入 PDK 特製化程序期間所需的任何其他檔案底部的按鈕。 比方說，如果您的自動安裝檔案會安裝到 VM 的 RDP 憑證 (如中所述[藉由新增 ShieldingDataAnswerFile 函式產生回應檔案](guarded-fabric-sample-unattend-xml-file.md))，您應該將 RDPCert.pfx 檔案中參考自動安裝檔案，請前往。 請注意，您在此處指定的任何檔案會自動複製到 c:\\temp\\上建立的 VM。 自動安裝檔案應該預期時，能夠在該資料夾路徑來參考這些檔案。

7.  檢閱您在下一步 頁面上的選項，然後按**產生**。

8.  完成後，請關閉精靈。

## <a name="create-a-shielding-data-file-and-add-guardians-using-powershell"></a>建立虛擬機器防護資料檔案，並加入使用 PowerShell 的守護者

作為替代防護資料檔案精靈 中，您可以執行[新增 ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/new-shieldingdatafile?view=win10-ps)建立防護資料檔案。

所有的虛擬機器防護資料檔案必須使用正確的擁有者和守護者的憑證來授權對受防護網狀架構執行受防護的 Vm 設定。
您可以檢查您是否安裝在本機執行任何 guardians [Get HgsGuardian](https://docs.microsoft.com/en-us/powershell/module/hgsclient/get-hgsguardian?view=win10-ps)。 擁有者 guardians 有私密金鑰，而您的資料中心的守護者通常不這麼做。

如果您需要建立擁有者守護者，請執行下列命令：

```powershell
New-HgsGuardian -Name "Owner" -GenerateCertificates
```

此命令會建立一組簽署和加密憑證的 「 受防護的 VM 本機憑證 」 資料夾下的 本機電腦的憑證存放區中。
您需要擁有者的憑證和其對應的私密金鑰 unshield 虛擬機器，因此請確定這些憑證可備份及保護防止遭竊。
擁有者憑證的存取權的攻擊者可以使用它們來啟動受防護的虛擬機器，或變更其安全性設定。

如果您要匯入從您想要用來執行您的虛擬機器 （您主要資料中心、 備份的資料中心等），執行下列命令，針對每個受防護網狀架構的守護者資訊[從您的受防護網狀架構擷取中繼資料檔案](#Select-trusted-fabrics).

```powershell
Import-HgsGuardian -Name 'EAST-US Datacenter' -Path '.\EastUSGuardian.xml'
```

> [!TIP]
> 如果您使用自我簽署的憑證或憑證向 HGS 已過期，您可能需要使用`-AllowUntrustedRoot`及/或`-AllowExpired`旗標來略過安全性檢查的匯入 HgsGuardian 命令。

您也必須[取得磁碟區簽章目錄](#Get-the-volume-signature-catalog-file)針對每個您想要使用與此虛擬機器防護資料檔案的範本磁碟和[防護資料回應檔案](#Create-an-answer-file)允許作業系統完成其特製化的工作自動。
最後，決定是否要讓您完全受防護或剛剛啟用 vTPM 的 VM。
使用`-Policy Shielded`完全受防護的 vm 或`-Policy EncryptionSupported`vTPM 啟用 允許基本的主控台連線的 VM 和 PowerShell Direct。

一切準備就緒之後，執行下列命令來建立防護資料檔案：

```powershell
$viq = New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\marketing-ws2016.vsc' -VersionRule Equals
New-ShieldingDataFile -ShieldingDataFilePath "C:\temp\Marketing-LBI.pdk" -Policy EncryptionSupported -Owner 'Owner' -Guardian 'EAST-US Datacenter' -VolumeIDQualifier $viq -AnswerFile 'C:\temp\marketing-ws2016-answerfile.xml'
```

在上述命令中，名為 「 擁有者 」 （取自 Get HgsGuardian） 守護者將能夠變更安全性設定的 VM 在未來，而 '美國東部資料中心' 可以執行 VM，但無法變更其設定。
如果您有一個以上的守護者時，個別使用逗號守護者的名稱類似`'EAST-US Datacenter', 'EMEA Datacenter'`。
磁碟區的識別碼辨識符號指定只確切版本 （等於） 的範本磁碟或未來的版本 (GreaterThanOrEquals) 以及您是否信任。
磁碟名稱和簽章憑證必須完全相符版本比較，以在部署階段考量。
您可以藉由提供逗號分隔的清單，磁碟區的 ID 限定詞信任一個以上的範本磁碟`-VolumeIDQualifier`參數。
最後，如果您需要將其他檔案隨附了 VM，也就是使用回應檔案需要`-OtherFile`參數，並提供以逗號分隔清單的檔案路徑。

請參閱 cmdlet 文件[新增 ShieldingDataFile](https://docs.microsoft.com/en-us/powershell/module/shieldedvmdatafile/New-ShieldingDataFile?view=win10-ps)並[新增 VolumeIDQualifier](https://docs.microsoft.com/en-us/powershell/module/shieldedvmdatafile/New-VolumeIDQualifier?view=win10-ps)若要深入了解設定防護資料檔案的其他方法。

## <a name="see-also"></a>另請參閱

- [部署受防護的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 Vm](guarded-fabric-and-shielded-vms-top-node.md)

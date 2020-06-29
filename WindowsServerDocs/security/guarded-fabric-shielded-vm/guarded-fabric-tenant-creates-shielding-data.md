---
title: 受防護的租使用者 Vm-建立防護資料以定義受防護的 VM
ms.prod: windows-server
ms.topic: article
ms.assetid: 49f4e84d-c1f7-45e5-9143-e7ebbb2ef052
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: 30f8f4db8f6bbfd4ead6ce2a31af3b2f6adbf72c
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475095"
---
# <a name="shielded-vms-for-tenants---creating-shielding-data-to-define-a-shielded-vm"></a>受防護的租使用者 Vm-建立防護資料以定義受防護的 VM

>適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

防護資料檔案 (也稱為佈建資料檔案或 PDK 檔案) 是一種加密檔案，租用戶或 VM 擁有者會建立該檔案以保護重要的 VM 組態資訊，例如系統管理員密碼、RDP 及其他身分識別相關的憑證、網域加入認證等等。 本主題提供如何建立防護資料檔案的相關資訊。 建立檔案之前，您必須從您的主機服務提供者取得範本磁片，或建立範本磁片，如租使用者的[受防護 vm-建立範本磁片（選擇性）](guarded-fabric-tenant-creates-template-disk.md)中所述。

如需防護資料檔案內容的清單和圖表，請參閱[什麼是防護資料，以及為何需要它？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)。

> [!IMPORTANT]
> 本節中的步驟應該在受防護網狀架構以外的另一部信任電腦上完成。 一般而言，VM 擁有者（租使用者）會建立其 Vm 的防護資料，而不是網狀架構系統管理員。

若要準備建立防護資料檔案，請執行下列步驟：

- [取得遠端桌面連線的憑證](#optional-obtain-a-certificate-for-remote-desktop-connection)
- [建立回應檔案](#create-an-answer-file)
- [取得磁片區簽章類別目錄檔案](#get-the-volume-signature-catalog-file)
- [選取信任的網狀架構](#select-trusted-fabrics)

接著，您可以建立防護資料檔案：

- [建立防護資料檔案並新增監護人](#create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard)

## <a name="optional-obtain-a-certificate-for-remote-desktop-connection"></a>選擇性取得遠端桌面連線的憑證

由於租使用者只能使用遠端桌面連線或其他遠端系統管理工具來連線到其受防護的 Vm，因此請務必確保租使用者可以驗證它們是否連線到正確的端點（也就是不會有「中間人」攔截連接）。

驗證您要連線到目標伺服器的其中一種方式，就是安裝和設定憑證，讓遠端桌面服務在您起始連線時顯示。 連接到伺服器的用戶端電腦將會檢查它是否信任憑證，如果沒有，則會顯示警告。 一般而言，若要確保連線的用戶端信任憑證，RDP 憑證會從租使用者的 PKI 發行。 如需如何[在遠端桌面服務中使用憑證](https://technet.microsoft.com/library/dn781533.aspx)的詳細資訊，請參閱 TechNet。

 為了協助您決定是否需要取得自訂的 RDP 憑證，請考慮下列事項：

- 如果您只是在實驗室環境中測試受防護的 Vm，就**不**需要自訂的 RDP 憑證。
- 如果您的 VM 已設定為加入 Active Directory 網域，則電腦憑證通常會自動由貴組織的憑證授權單位單位發行，並在 RDP 連線期間用來識別電腦。 您**不**需要自訂的 RDP 憑證。
- 如果您的 VM 未加入網域，但您想要在使用遠端桌面時驗證連線到正確電腦的方法，您**應該考慮**使用自訂的 RDP 憑證。

> [!TIP]
> 選取要包含在防護資料檔案中的 RDP 憑證時，請務必使用萬用字元憑證。 一個防護資料檔案可用來建立不限數目的 Vm。 由於每個 VM 會共用相同的憑證，因此不論 VM 的主機名稱為何，萬用字元憑證都會確保憑證有效。

## <a name="create-an-answer-file"></a>建立回應檔案

由於 VMM 中已簽署的範本磁片已一般化，因此租使用者必須提供回應檔案，以在布建過程中將其受防護的 Vm 特殊化。 回應檔案（通常稱為自動安裝檔案）可以針對其預定的角色設定 VM，也就是它可以安裝 Windows 功能、註冊在上一個步驟中建立的 RDP 憑證，以及執行其他自訂動作。 它也會提供 Windows 安裝程式所需的資訊，包括預設系統管理員的密碼和產品金鑰。

如需取得和使用**ShieldingDataAnswerFile**函式來產生回應檔案（Unattend.xml 檔案）以建立受防護 vm 的詳細資訊，請參閱[使用 ShieldingDataAnswerFile 函式產生回應](guarded-fabric-sample-unattend-xml-file.md)檔案。 使用函式，您可以更輕鬆地產生回應檔案，以反映如下所示的選項：

- VM 是否打算在初始化程式結束時加入網域？
- 您將使用每個 VM 的大量授權或特定產品金鑰嗎？
- 您使用的是 DHCP 或靜態 IP 嗎？
- 您將使用自訂的遠端桌面通訊協定（RDP）憑證來證明 VM 是否屬於您的組織？
- 您想要在初始化結束時執行腳本嗎？

系統會在每個使用該防護資料檔案建立的 VM 上使用防護資料檔案中所使用的回應檔案。 因此，您應該確定您不會將任何 VM 特定的資訊硬式編碼到回應檔案中。 VMM 支援自動安裝檔案中的一些替代字串（請參閱下表），以處理可能會從 VM 變更為 VM 的特製化值。 您不需要使用這些值;不過，如果它們存在，VMM 將會利用它們。

為受防護的 Vm 建立 unattend.xml 檔案時，請記住下列限制：

- 如果您使用 VMM 來管理您的資料中心，自動安裝檔案必須在設定好之後關閉 VM。 這是為了讓 VMM 知道 VM 何時應向租使用者報告，以供 VM 完成布建並準備好可供使用。 VMM 會在虛擬機器偵測到布建期間已關閉後，自動啟動 VM。

- 請務必啟用 RDP 和對應的防火牆規則，以便您可以在 VM 設定完成後加以存取。 您不能使用 VMM 主控台來存取受防護的 Vm，因此您將需要 RDP 來連線到您的 VM。 如果您想要使用 Windows PowerShell 遠端處理來管理您的系統，請確定已啟用 WinRM。

- 受防護的 VM 自動安裝檔案中唯一支援的替代字串如下：

    | 可取代的元素 | 替代字串 |
    |-----------|-----------|
    | ComputerName        | @ComputerName@      |
    | TimeZone            | @TimeZone@          |
    | ProductKey          | @ProductKey@        |
    | IPAddr4-1           | @IP4Addr-1@         |
    | IPAddr6-1           | @IP6Addr-1@         |
    | MACAddr-1           | @MACAddr-1@         |
    | 前置詞-1-1          | @Prefix-1-1@        |
    | NextHop-1-1         | @NextHop-1-1@       |
    | 前置詞-1-2          | @Prefix-1-2@        |
    | NextHop-1-2         | @NextHop-1-2@       |

    如果您有多個 NIC，您可以藉由遞增第一個數位來新增多個 IP 設定的替代字串。 例如，若要設定2個 Nic 的 IPv4 位址、子網和閘道，您可以使用下列替代字串：

    | 替代字串 | 替代範例 |
    |---------------------|----------------------|
    | @IP4Addr-1@         | 192.168.1.10/24      |
    | @MACAddr-1@         | 乙太網路             |
    | @Prefix-1-1@        | 24                   |
    | @NextHop-1-1@       | 192.168.1.254        |
    | @IP4Addr-2@         | 10.0.20.30/24        |
    | @MACAddr-2@         | Ethernet 2           |
    | @Prefix-2-1@        | 24                   |
    | @NextHop-2-1@       | 10.0.20.1            |

使用替代字串時，請務必確定在 VM 布建程式期間將會填入字串。 如果 @ProductKey 在部署時未提供 @ 之類的字串，將自動安裝檔案 &lt; 中的 ProductKey &gt; 節點保留空白，則特製化程式將會失敗，而且您將無法連線到您的 VM。

此外，請注意，只有在您利用 VMM 靜態 IP 位址池時，才會使用與資料表結尾相對應的網路相關替代字串。 您的主機服務提供者應該能夠告訴您是否需要這些替代字串。 如需 VMM 範本中靜態 IP 位址的詳細資訊，請參閱 VMM 檔中的下列內容：

- [IP 位址集區的指導方針](https://technet.microsoft.com/system-center-docs/vmm/plan/plan-network#guidelines-for-ip-address-pools)
- [設定 VMM 光纖中的靜態 IP 位址集區](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-network-static-address-pools)

最後，請務必注意，受防護的 VM 部署程式只會加密 OS 磁片磁碟機。 如果您部署具有一或多個資料磁片磁碟機的受防護 VM，強烈建議您在租使用者網域中新增自動安裝命令或群組原則設定，以自動加密資料磁片磁碟機。

## <a name="get-the-volume-signature-catalog-file"></a>取得磁片區簽章類別目錄檔案

防護資料檔案也包含租使用者所信任之範本磁片的相關資訊。 租使用者會以磁片區簽章類別目錄（VSC）檔案的形式，從受信任的範本磁片取得磁片簽章。 當部署新的 VM 時，就會驗證這些簽章。 如果防護資料檔案中沒有任何簽章符合嘗試與 VM 一起部署的範本磁片（亦即，它已修改或以不同的潛在惡意磁片進行交換），布建程式將會失敗。

> [!IMPORTANT]
> 雖然 VSC 可確保磁片不會遭到篡改，但租使用者一開始就必須信任該磁片。 如果您是租使用者，且範本磁片是由主機服務提供者所提供，請使用該範本磁片部署測試 VM，並執行您自己的工具（防毒軟體、弱點掃描器等等），以驗證磁片在您信任的狀態中。

有兩種方式可以取得範本磁片的 VSC：

1. 主機服務提供者（或租使用者，如果租使用者具有 VMM 的存取權）會使用 VMM PowerShell Cmdlet 來儲存 VSC，並將其授與給租使用者。 這可以在已安裝並設定 VMM 主控台的任何電腦上執行，以管理主控網狀架構的 VMM 環境。 用來儲存 VSC 的 PowerShell Cmdlet 包括：

    ```powershell
    $disk = Get-SCVirtualHardDisk -Name "templateDisk.vhdx"

    $vsc = Get-SCVolumeSignatureCatalog -VirtualHardDisk $disk

    $vsc.WriteToFile(".\templateDisk.vsc")
    ```

2. 租使用者可存取範本磁片檔案。 如果租使用者建立一個要上傳至主機服務提供者的範本磁片，或租使用者可以下載主機服務提供者的範本磁片，就可能發生這種情況。 在此情況下，在圖片中沒有 VMM，租使用者會執行下列 Cmdlet （以受防護的 VM 工具功能安裝，遠端伺服器管理工具的一部分）：

    ```powershell
    Save-VolumeSignatureCatalog -TemplateDiskPath templateDisk.vhdx -VolumeSignatureCatalogPath templateDisk.vsc
    ```

## <a name="select-trusted-fabrics"></a>選取信任的網狀架構

防護資料檔案中的最後一個元件與 VM 的擁有者和監護人相關。 監護人會用來指定受防護 VM 的擁有者，以及其授權執行的受保護網狀架構。

若要授權主控網狀架構執行受防護的 VM，您必須從主控服務提供者的主機守護者服務取得守護者中繼資料。 主機服務提供者通常會透過其管理工具為您提供此中繼資料。 在企業案例中，您可以直接存取以自行取得中繼資料。

您或您的主機服務提供者可以藉由執行下列其中一個動作，從 HGS 取得守護者中繼資料：

- 執行下列 Windows PowerShell 命令，或流覽至網站並儲存所顯示的 XML 檔案，直接從 HGS 取得守護者中繼資料：

    ```powershell
    Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml
    ```

- 使用 VMM PowerShell Cmdlet，從 VMM 取得守護者中繼資料：

    ```powershell
    $relecloudmetadata = Get-SCGuardianConfiguration
    $relecloudmetadata.InnerXml | Out-File .\RelecloudGuardian.xml -Encoding UTF8
    ```

取得您想要授權受防護的 Vm 執行的每個受保護網狀架構的守護者中繼資料檔案，然後再繼續。

## <a name="create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard"></a>建立防護資料檔案，並使用防護資料檔案嚮導來新增保護者

執行 [防護資料檔案嚮導] 以建立防護資料（PDK）檔案。 在這裡，您將新增 RDP 憑證、自動安裝檔案、磁片區簽章目錄、擁有者防護，以及在上一個步驟中取得的下載的守護者中繼資料。

1. 使用伺服器管理員或下列 Windows PowerShell 命令，在您的電腦上安裝**遠端伺服器管理工具 &gt; 功能管理工具 &gt; 受防護的 VM 工具**：

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

2. 從 [開始] 功能表上的 [系統管理員工具] 區段，或藉由執行下列**C： \\ Windows \\ System32 \\ShieldingDataFileWizard.exe**，開啟 [防護資料檔案]。

3. 在第一個頁面上，使用第二個 [檔案選擇] 方塊，為您的防護資料檔案選擇位置和檔案名。 一般來說，您會在擁有以該防護資料（例如，HR、IT、財務）及其執行的工作負載角色（例如，檔案伺服器、網頁伺服器或自動安裝檔案所設定的任何專案）所建立的任何 Vm 的實體之後，將防護資料檔案命名為。 將 [**受防護的範本的資料**] 選項按鈕設定為 [防護資料]。

    > [!NOTE]
    > 在 [防護資料檔案] 中，您會看到下列兩個選項：
    >- **受防護範本的防護資料**
    >- **現有 Vm 和未受防護範本的防護資料**<br>
    > 第一個選項是在從受防護的範本建立新的受防護 Vm 時使用。 第二個選項可讓您建立只能在轉換現有 Vm 或從未受防護的範本建立受防護的 Vm 時使用的防護資料。

    ![防護資料檔案嚮導，檔案選取](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-01.png)

    此外，您還必須選擇使用此防護資料檔案建立的 Vm 是否會真正受到防護，或在「支援加密」模式下設定。 如需這兩個選項的詳細資訊，請參閱受防護網狀[架構可以執行哪些類型的虛擬機器？](guarded-fabric-and-shielded-vms.md#what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run)。

    > [!IMPORTANT]
    > 請小心注意下一個步驟，因為它會定義受防護 Vm 的擁有者，以及受防護的 Vm 將獲授權執行的網狀架構。<br>若要在稍後將現有的受防護 VM 從受**防護**變更為**支援的加密**，或反之亦然，必須擁有**擁有者**守護程式。

4. 您在此步驟中的目標是兩折迭：

    - 建立或選取代表您身為 VM 擁有者的擁有者

    - 匯入您在上一個步驟中從主控提供者（或您自己的）主機守護者服務下載的守護者

    若要指定現有的擁有者守護者，請從下拉式功能表中選取適當的 [守護者]。 此清單中只會顯示本機電腦上已安裝私密金鑰的監護人。 您也可以選取右下角的 [**管理本機**保護者]，然後按一下 [**建立**並完成嚮導]，來建立自己的擁有者守護者。

    接下來，我們會使用 [**擁有者和監護人**] 頁面，再次匯入稍早下載的守護程式中繼資料。 選取右下角的 [**管理本機**保護者]。 使用匯**入**功能匯入守護者中繼資料檔案。 匯入或新增所有必要的監護人之後，請按一下 **[確定]** 。 最佳作法是在主機服務提供者或企業資料中心之後，將監護人命名為。 最後，選取所有代表受防護 VM 獲授權執行之資料中心的所有監護人。 您不需要再次選取 [擁有者]。 完成後，請按 **[下一步]** 。

    ![防護資料檔案嚮導、擁有者和監護人](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-02.png)

5. 在 [磁片區識別碼限定詞] 頁面上，按一下 [**新增**]，在您的防護資料檔案中授權已簽署的範本磁片。 當您在對話方塊中選取 VSC 時，它會顯示該磁片的名稱、版本，以及用來簽署它的憑證的相關資訊。 針對每個您想要授權的範本磁片重複此程式。

6. 在 [特製**化值**] 頁面上，按一下 **[流覽]** 以選取將用來將您的 vm 特殊化的 unattend.xml 檔案。

    使用底部的 [**新增**] 按鈕，將任何額外的檔案新增至特製化程式期間所需的 PDK。 例如，如果您的自動安裝檔案會將 RDP 憑證安裝到 VM （如[使用 ShieldingDataAnswerFile 函式產生回應](guarded-fabric-sample-unattend-xml-file.md)檔案中所述），您應該在這裡新增 RDP 憑證 PFX 檔案和 RDPCertificateConfig.ps1 腳本。 請注意，您在此處指定的任何檔案，都會 \\ \\ 在建立的 VM 上自動複製到 C： temp。 當您的自動安裝檔案依路徑參考檔案時，應該會預期該資料夾中的檔案。

7. 在下一個頁面上檢查您的選擇，然後按一下 [**產生**]。

8. 完成後，請關閉嚮導。

## <a name="create-a-shielding-data-file-and-add-guardians-using-powershell"></a>使用 PowerShell 建立防護資料檔案並新增監護人

除了 [防護資料檔案] [檔案] 之外，您還可以執行[ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/new-shieldingdatafile?view=win10-ps)來建立防護資料檔案。

所有的防護資料檔案都必須使用正確的擁有者和監護人憑證進行設定，以授權受防護的虛擬機器在受保護網狀架構上執行。
您可以藉由執行[HgsGuardian](https://docs.microsoft.com/powershell/module/hgsclient/get-hgsguardian?view=win10-ps)來檢查是否已在本機安裝任何監護人。 擁有者的監護人擁有私密金鑰，而您的資料中心的守護者通常不會這麼做。

如果您需要建立擁有者守護者，請執行下列命令：

```powershell
New-HgsGuardian -Name "Owner" -GenerateCertificates
```

此命令會在本機電腦的憑證存放區中，于 [受防護的 VM 本機憑證] 資料夾底下建立一對簽署和加密憑證。
您將需要擁有者憑證及其對應的私密金鑰，才能 unshield 虛擬機器，因此請確定這些憑證已備份且受到保護而無法遭竊。
具有擁有者憑證存取權的攻擊者可以使用它們來啟動受防護的虛擬機器，或變更其安全性設定。

如果您需要從要執行虛擬機器的受防護網狀架構（主要資料中心、備份資料中心等）匯入守護者資訊，請針對每個[從您的受](#select-trusted-fabrics)防護網狀架構取得的中繼資料檔案執行下列命令。

```powershell
Import-HgsGuardian -Name 'EAST-US Datacenter' -Path '.\EastUSGuardian.xml'
```

> [!TIP]
> 如果您使用自我簽署憑證，或向 HGS 註冊的憑證已過期，您可能需要使用 `-AllowUntrustedRoot` 和/或 `-AllowExpired` 旗標搭配 HgsGuardian 命令來略過安全性檢查。

您也需要為每個要與此防護資料檔案搭配使用的範本磁片[取得磁片](#get-the-volume-signature-catalog-file)區簽章目錄，並針對[防護資料回應](#create-an-answer-file)檔案，讓作業系統能夠自動完成其特製化工作。
最後，請決定您要讓 VM 受到完全防護或僅 vTPM 啟用。
用於 `-Policy Shielded` 完全受防護的 vm `-Policy EncryptionSupported` ，或用於允許基本主控台連線和 PowerShell Direct 的已啟用 vTPM vm。

一切就緒後，請執行下列命令來建立您的防護資料檔案：

```powershell
$viq = New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\marketing-ws2016.vsc' -VersionRule Equals
New-ShieldingDataFile -ShieldingDataFilePath "C:\temp\Marketing-LBI.pdk" -Policy EncryptionSupported -Owner 'Owner' -Guardian 'EAST-US Datacenter' -VolumeIDQualifier $viq -AnswerFile 'C:\temp\marketing-ws2016-answerfile.xml'
```

> [!TIP]
> 如果您使用的是自訂的 RDP 憑證、SSH 金鑰或其他需要隨附于防護資料檔案的檔案，請使用 `-OtherFile` 參數來包含這些檔案。 您可以提供以逗號分隔的檔案路徑清單，例如`-OtherFile "C:\source\myRDPCert.pfx", "C:\source\RDPCertificateConfig.ps1"`

在上述命令中，名為 "Owner" 的守護者（從 HgsGuardian 取得）將能夠在未來變更 VM 的安全性設定，而「美國東部資料中心」則可以執行 VM，但不能變更其設定。
如果您有一個以上的守護者，請使用逗號來分隔監護人的名稱，例如 `'EAST-US Datacenter', 'EMEA Datacenter'` 。
磁片區識別碼辨識符號會指定您是否也只信任範本磁片或未來版本（GreaterThanOrEquals）的確切版本（Equals）。
磁片名稱和簽署憑證必須完全符合，才能在部署階段考慮版本比較。
您可以藉由將以逗號分隔的磁片區識別碼辨識符號清單提供給參數，來信任一個以上的範本磁片 `-VolumeIDQualifier` 。
最後，如果您有其他需要隨 VM 一起回應檔案的檔案，請使用 `-OtherFile` 參數並提供以逗號分隔的檔案路徑清單。

請參閱[ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-ShieldingDataFile?view=win10-ps)和[VolumeIDQualifier](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-VolumeIDQualifier?view=win10-ps)的 Cmdlet 檔，以瞭解設定防護資料檔案的其他方式。

## <a name="additional-references"></a>其他參考

- [部署受防護的 VM](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)

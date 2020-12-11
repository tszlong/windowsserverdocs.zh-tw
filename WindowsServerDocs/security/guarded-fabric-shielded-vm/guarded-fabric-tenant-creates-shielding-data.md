---
title: 租使用者的受防護 Vm-建立防護資料以定義受防護的 VM
description: 深入瞭解：租使用者的受防護 Vm-建立防護資料以定義受防護的 VM
ms.topic: article
ms.assetid: 49f4e84d-c1f7-45e5-9143-e7ebbb2ef052
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 09/25/2019
ms.openlocfilehash: 1b45f94ac5c63dcae5885c61cc1099327f17b852
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043856"
---
# <a name="shielded-vms-for-tenants---creating-shielding-data-to-define-a-shielded-vm"></a>租使用者的受防護 Vm-建立防護資料以定義受防護的 VM

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

防護資料檔案 (也稱為佈建資料檔案或 PDK 檔案) 是一種加密檔案，租用戶或 VM 擁有者會建立該檔案以保護重要的 VM 組態資訊，例如系統管理員密碼、RDP 及其他身分識別相關的憑證、網域加入認證等等。 本主題提供如何建立防護資料檔案的相關資訊。 建立檔案之前，您必須從主機服務提供者取得範本磁片，或建立範本磁片（如租使用者的 [受防護 vm 所述-建立範本磁片 (選用) ](guarded-fabric-tenant-creates-template-disk.md)。

如需防護資料檔案內容的清單和圖表，請參閱 [何謂防護資料，以及為何需要它？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)。

> [!IMPORTANT]
> 本節中的步驟應該在受防護網狀架構之外的不同受信任電腦上完成。 一般而言，VM 擁有者 (租使用者) 會建立其 Vm 的防護資料，而不是網狀架構系統管理員。

若要準備建立防護資料檔案，請執行下列步驟：

- [取得遠端桌面連線的憑證](#optional-obtain-a-certificate-for-remote-desktop-connection)
- [建立回應檔案](#create-an-answer-file)
- [取得磁片區簽章目錄檔案](#get-the-volume-signature-catalog-file)
- [選取信任的網狀架構](#select-trusted-fabrics)

然後，您可以建立防護資料檔案：

- [建立防護資料檔案並新增監護人](#create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard)

## <a name="optional-obtain-a-certificate-for-remote-desktop-connection"></a> (選用) 取得遠端桌面連線的憑證

因為租使用者只能使用遠端桌面連線或其他遠端系統管理工具連接到其受防護的 Vm，所以請務必確定租使用者可以確認它們是否連線到正確的 (端點，而不會有「中間人」攔截連接) 。

驗證您要連線到預期伺服器的其中一種方式，是安裝和設定憑證，讓遠端桌面服務在您起始連線時顯示。 連接到伺服器的用戶端電腦會檢查它是否信任憑證，並顯示警告（如果沒有的話）。 一般而言，為了確保連線的用戶端會信任憑證，會從租使用者的 PKI 發出 RDP 憑證。 如需 [在遠端桌面服務中使用憑證的](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn781533(v=ws.11)) 詳細資訊，請參閱 TechNet 上的。

 為協助您決定是否需要取得自訂 RDP 憑證，請考慮下列事項：

- 如果您只是在實驗室環境中測試受防護的 Vm，就 **不** 需要自訂 RDP 憑證。
- 如果您的 VM 設定為加入 Active Directory 網域，則電腦憑證通常會由您組織的憑證授權單位單位自動發行，並在 RDP 連線期間用來識別電腦。 您 **不** 需要自訂 RDP 憑證。
- 如果您的 VM 未加入網域，但您想要在使用遠端桌面時驗證您是否連線到正確電腦的方法，您 **應該考慮** 使用自訂 RDP 憑證。

> [!TIP]
> 當您選取要包含在防護資料檔案中的 RDP 憑證時，請務必使用萬用字元憑證。 您可以使用一個防護資料檔案來建立不限數目的 Vm。 因為每個 VM 都會共用相同的憑證，所以無論 VM 的主機名稱為何，萬用字元憑證都會確保憑證有效。

## <a name="create-an-answer-file"></a>建立回應檔案

由於 VMM 中已簽署的範本磁片已一般化，因此租使用者必須提供回應檔案，以便在布建過程中特製化其受防護的 Vm。 回應檔案 (通常稱為自動安裝檔案) 可以為其預定角色設定 VM，也就是它可以安裝 Windows 功能、註冊在上一個步驟中建立的 RDP 憑證，以及執行其他自訂動作。 它也會提供 Windows 安裝程式所需的資訊，包括預設系統管理員的密碼和產品金鑰。

如需取得和使用 **ShieldingDataAnswerFile** 函式來產生回應檔案 ( # A0 檔案) 來建立受防護的 vm 的詳細資訊，請參閱 [使用 New-ShieldingDataAnswerFile 函數產生回應](guarded-fabric-sample-unattend-xml-file.md)檔案。 使用函式時，您可以更輕鬆地產生回應檔案，以反映下列選項：

- VM 是否打算在初始化程式結束時加入網域？
- 您是否會使用每個 VM 的大量授權或特定產品金鑰？
- 您是使用 DHCP 還是靜態 IP？
- 您是否會使用自訂遠端桌面通訊協定 (RDP) 憑證，以用來證明 VM 屬於您的組織嗎？
- 您是否要在初始化結束時執行腳本？

用於防護資料檔案中的回應檔案，將用於使用該防護資料檔案建立的每個 VM 上。 因此，您應該確定不會將任何 VM 特定的資訊硬式編碼到回應檔案中。 VMM 支援一些替代字串 (請參閱下表中) 的自動安裝檔案，以處理可能會從 VM 變更為 VM 的特製化值。 您不需要使用這些;但是，如果它們存在，VMM 將會利用它們。

針對受防護的 Vm 建立 unattend.xml 檔案時，請記住下列限制：

- 如果您使用 VMM 來管理您的資料中心，自動安裝檔案必須在設定好之後關閉 VM。 這是為了讓 VMM 知道何時應該向租使用者報告 VM 完成布建並準備好可供使用。 當 VM 偵測到在布建期間已關閉時，VMM 會自動重新開機 VM。

- 請務必啟用 RDP 和對應的防火牆規則，以便您可以在設定 VM 之後存取 VM。 您無法使用 VMM 主控台來存取受防護的 Vm，因此您將需要 RDP 連線到您的 VM。 如果您想要使用 Windows PowerShell 遠端管理系統，請確定已啟用 WinRM。

- 受防護的 VM 自動安裝檔案中唯一支援的替代字串如下：

    | 可替換元素 | 替代字串 |
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

    如果您有一個以上的 NIC，您可以藉由遞增第一個數位，為 IP 設定新增多個替代字串。 例如，若要設定2個 Nic 的 IPv4 位址、子網和閘道，您可以使用下列替代字串：

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

使用替代字串時，請務必確定在 VM 布建過程中會填入字串。 如果 @ProductKey 在部署期間未提供如 @ 的字串，則將自動安裝檔案 &lt; 中的 ProductKey &gt; 節點保留空白，特製化程式將會失敗，且您將無法連線到您的 VM。

此外，請注意，只有當您利用 VMM 靜態 IP 位址池時，才會使用與資料表結尾相關的網路相關替代字串。 您的主機服務提供者應該能夠告訴您是否需要這些替代字串。 如需 VMM 範本中靜態 IP 位址的詳細資訊，請參閱 VMM 檔中的下列內容：

- [IP 位址集區的指導方針](https://technet.microsoft.com/system-center-docs/vmm/plan/plan-network#guidelines-for-ip-address-pools)
- [設定 VMM 光纖中的靜態 IP 位址集區](/system-center/vmm/network-pool)

最後一點要注意的是，受防護的 VM 部署程式只會加密 OS 磁片磁碟機。 如果您使用一或多個資料磁片磁碟機來部署受防護的 VM，強烈建議您在租使用者網域中新增自動安裝命令或群組原則設定，以自動加密資料磁片磁碟機。

## <a name="get-the-volume-signature-catalog-file"></a>取得磁片區簽章目錄檔案

防護資料檔案也包含租使用者所信任之範本磁片的相關資訊。 租使用者會從磁片區簽章目錄的形式取得受信任範本磁片的磁片簽章， (VSC) 檔。 這些簽章接著會在部署新的 VM 時進行驗證。 如果防護資料檔中的簽章都不符合嘗試使用 VM 部署的範本磁片 (也就是它已修改或交換為不同的可能惡意磁片) ，布建程式將會失敗。

> [!IMPORTANT]
> 雖然 VSC 可確保磁片未遭篡改，但租使用者一開始就必須信任該磁片。 如果您是主機服務提供者，且範本磁片是由您的主機服務提供者所提供，請使用該範本磁片來部署測試 VM，並執行您自己的工具 (的防毒軟體、弱點掃描程式等) 來驗證磁片實際上是在您信任的狀態。

有兩種方式可以取得範本磁片的 VSC：

1. 主機服務提供者 (或租使用者，如果租使用者可存取 VMM) 使用 VMM PowerShell Cmdlet 來儲存 VSC 並將它提供給租使用者。 您可以在任何已安裝並設定 VMM 主控台的電腦上執行此操作，以管理主控網狀架構的 VMM 環境。 用來儲存 VSC 的 PowerShell Cmdlet 如下：

    ```powershell
    $disk = Get-SCVirtualHardDisk -Name "templateDisk.vhdx"

    $vsc = Get-SCVolumeSignatureCatalog -VirtualHardDisk $disk

    $vsc.WriteToFile(".\templateDisk.vsc")
    ```

2. 租使用者具有範本磁片檔案的存取權。 如果租使用者建立要上傳至主機服務提供者的範本磁片，或是租使用者可以下載主機服務提供者的範本磁片，就可能會發生這種情況。 在此情況下，如果沒有 VMM，租使用者會執行下列 Cmdlet (與受防護的 VM 工具功能一起安裝，遠端伺服器管理工具) 的一部分：

    ```powershell
    Save-VolumeSignatureCatalog -TemplateDiskPath templateDisk.vhdx -VolumeSignatureCatalogPath templateDisk.vsc
    ```

## <a name="select-trusted-fabrics"></a>選取信任的網狀架構

防護資料檔案中的最後一個元件與 VM 的擁有者和守護者相關。 守護者可用來指定受防護 VM 的擁有者，以及已獲授權執行的受防護網狀架構。

若要授權主機網狀架構執行受防護的 VM，您必須從主機服務提供者的主機守護者服務取得守護者中繼資料。 主機服務提供者通常會透過其管理工具提供此中繼資料。 在企業案例中，您可以直接存取來自行取得中繼資料。

您或您的主機服務提供者可以藉由執行下列其中一項動作，從 HGS 取得守護者中繼資料：

- 藉由執行下列 Windows PowerShell 命令，或流覽至網站並儲存所顯示的 XML 檔案，直接從 HGS 取得守護者中繼資料：

    ```powershell
    Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml
    ```

- 使用 VMM PowerShell Cmdlet 從 VMM 取得守護者中繼資料：

    ```powershell
    $relecloudmetadata = Get-SCGuardianConfiguration
    $relecloudmetadata.InnerXml | Out-File .\RelecloudGuardian.xml -Encoding UTF8
    ```

針對您想要授權受防護 Vm 執行的每個受防護網狀架構，取得守護者中繼資料檔案，然後再繼續進行。

## <a name="create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard"></a>使用防護資料檔案 wizard 建立防護資料檔案並新增監護人

執行 [防護資料檔案] wizard，以 (PDK) 檔建立防護資料。 在這裡，您將新增 RDP 憑證、自動安裝檔案、磁片區簽章目錄、擁有者守護者，以及在上一個步驟中取得的下載的守護者中繼資料。

1. 使用伺服器管理員或下列 Windows PowerShell 命令，在您的電腦上安裝 **遠端伺服器管理工具 &gt; 功能管理工具 &gt; 防護的 VM 工具** ：

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

2. 從您 [開始] 功能表上的 [系統管理員工具] 區段開啟 [防護資料檔案嚮導]，或執行下列可執行檔 **C： \\ Windows \\ System32 \\ShieldingDataFileWizard.exe**。

3. 在第一個頁面上，使用第二個檔案選擇方塊來選擇您的防護資料檔案的位置和檔案名。 一般情況下，您會在擁有以該防護資料建立之任何 Vm 的實體之後，將防護資料檔命名 (例如，HR、IT、財務) 和執行的工作負載角色 (例如，檔案伺服器、網頁伺服器或自動安裝檔案) 所設定的其他任何 Vm。 將選項按鈕設定為 [ **防護受防護範本的資料**]。

    > [!NOTE]
    > 在 [防護資料檔案] 中，您會看到下列兩個選項：
    >- **受防護範本的防護資料**
    >- **現有 Vm 和未受防護範本的防護資料**<br>
    > 第一個選項是在從受防護的範本建立新的受防護 Vm 時使用。 第二個選項可讓您建立防護資料，只有在轉換現有的 Vm 或從非受防護的範本建立受防護的 Vm 時才可使用。

    ![防護資料檔案嚮導，檔案選取](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-01.png)

    此外，您必須選擇使用此防護資料檔案建立的 Vm 是否會在「支援加密」模式中真正受到防護或設定。 如需這兩個選項的詳細資訊，請參閱 [哪些是受防護網狀架構可執行檔虛擬機器類型？](guarded-fabric-and-shielded-vms.md#what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run)。

    > [!IMPORTANT]
    > 請注意下一個步驟，因為它會定義受防護 Vm 的擁有者，以及您受防護的 Vm 將獲授權執行的網狀架構。<br>需要擁有 **擁有者守護者** ，才能在稍後將現有受防護的 VM 從 **防護** 變更為 **支援的加密** ，反之亦然。

4. 您在此步驟中的目標是兩個折迭：

    - 建立或選取代表您作為 VM 擁有者的擁有者

    - 匯入您在上一個步驟中從主機服務提供者的 (或您自己的) 主機守護者服務下載的守護者

    若要指定現有的擁有者守護者，請從下拉式功能表中選取適當的守護者。 只有安裝在本機電腦上且私密金鑰保持不變的保護者，才會顯示在此清單中。 您也可以選取右下角的 [ **管理本機** 保護者]，然後按一下 [ **建立** ] 和 [完成嚮導]，以建立自己的擁有者守護者。

    接下來，我們會使用 [ **擁有者和監護人** ] 頁面再次匯入先前下載的守護者中繼資料。 選取右下角的 [ **管理本機監護人** ]。 使用匯 **入** 功能匯入守護者中繼資料檔案。 匯入或新增所有必要的保護程式之後，請按一下 **[確定]** 。 最佳做法是在主機服務提供者或企業資料中心代表之後，將監護人命名為。 最後，選取代表受防護 VM 執行授權的資料中心的所有監護人。 您不需要再次選取「擁有者」。 完成之後，請按 **[下一步]** 。

    ![防護資料檔案嚮導、擁有者和保護者](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-02.png)

5. 在 [磁片區識別碼辨識符號] 頁面上，按一下 [ **新增** ]，在您的防護資料檔案中授權已簽署的範本磁片。 當您在對話方塊中選取 VSC 時，它會顯示該磁片的名稱、版本，以及用來簽署該磁片之憑證的相關資訊。 針對您想要授權的每個範本磁片重複此程式。

6. 在 [特製 **化值** ] 頁面上，按一下 **[流覽]** ，選取將用來特製化 vm 的 unattend.xml 檔案。

    使用底部的 [ **新增** ] 按鈕，將任何額外的檔案新增至特製化程式期間所需的 PDK。 例如，如果您的自動安裝檔案正在將 RDP 憑證安裝到 VM (如 [使用 New-ShieldingDataAnswerFile 函式產生回應](guarded-fabric-sample-unattend-xml-file.md) 檔案所述) ，您應該在這裡新增 RDP 憑證 PFX 檔案和 RDPCertificateConfig.ps1 腳本。 請注意，您在此處指定的任何檔案將會自動複製 \\ 到 \\ 建立的 VM 上的 C： temp。 當您的自動安裝檔案依路徑參考這些檔案時，該檔案應該會在該資料夾中。

7. 在下一個頁面上檢查您的選擇，然後按一下 [ **產生**]。

8. 完成之後，請關閉嚮導。

## <a name="create-a-shielding-data-file-and-add-guardians-using-powershell"></a>使用 PowerShell 建立防護資料檔案並新增監護人

您可以執行 [ShieldingDataFile](/powershell/module/shieldedvmdatafile/new-shieldingdatafile) 來建立防護資料檔案，做為防護資料檔案的替代方案。

所有防護資料檔案都必須使用正確的擁有者和守護者憑證來設定，以授權受防護的 Vm 可在受防護網狀架構上執行。
您可以藉由執行 [HgsGuardian](/powershell/module/hgsclient/get-hgsguardian)來檢查是否已在本機安裝任何監護人。 擁有者擁有者具有私密金鑰，而您的資料中心的守護者通常不會。

如果您需要建立擁有者守護者，請執行下列命令：

```powershell
New-HgsGuardian -Name "Owner" -GenerateCertificates
```

此命令會在本機電腦的憑證存放區中，于 [受防護的 VM 本機憑證] 資料夾下建立一組簽署和加密憑證。
您將需要擁有者憑證及其對應的私密金鑰來 unshield 虛擬機器，因此請確定這些憑證已備份並受到保護，以免遭竊。
擁有擁有者憑證存取權的攻擊者可以使用它們來啟動受防護的虛擬機器，或變更其安全性設定。

如果您需要從您想要執行虛擬機器的受防護網狀架構匯入守護者資訊 (您的主要資料中心、備份資料中心等 ) ，請針對每個 [從受](#select-trusted-fabrics)防護網狀架構取出的中繼資料檔案，執行下列命令。

```powershell
Import-HgsGuardian -Name 'EAST-US Datacenter' -Path '.\EastUSGuardian.xml'
```

> [!TIP]
> 如果您使用自我簽署憑證，或以 HGS 註冊的憑證已過期，您可能需要搭配 `-AllowUntrustedRoot` Import-HgsGuardian 命令使用 and/or `-AllowExpired` 旗標，以略過安全性檢查。

您也需要為每個您想要搭配此防護資料檔案和[防護資料回應](#create-an-answer-file)檔案使用的範本磁片取得磁片區簽章[目錄](#get-the-volume-signature-catalog-file)，以允許作業系統自動完成其特製化工作。
最後，決定是否要讓 VM 受到完整防護或僅 vTPM 啟用。
`-Policy Shielded`適用于完全受防護的 vm `-Policy EncryptionSupported` ，或適用于允許基本主控台連線和 PowerShell Direct 的啟用 VTPM 的 vm。

一切就緒後，請執行下列命令來建立您的防護資料檔案：

```powershell
$viq = New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\marketing-ws2016.vsc' -VersionRule Equals
New-ShieldingDataFile -ShieldingDataFilePath "C:\temp\Marketing-LBI.pdk" -Policy EncryptionSupported -Owner 'Owner' -Guardian 'EAST-US Datacenter' -VolumeIDQualifier $viq -AnswerFile 'C:\temp\marketing-ws2016-answerfile.xml'
```

> [!TIP]
> 如果您使用的是自訂 RDP 憑證、SSH 金鑰或其他需要包含在防護資料檔案中的檔案，請使用 `-OtherFile` 參數來包含它們。 您可以提供以逗號分隔的檔案路徑清單，如下所示： `-OtherFile "C:\source\myRDPCert.pfx", "C:\source\RDPCertificateConfig.ps1"`

在上述命令中，名為 "Owner" (從 HgsGuardian) 取得的守護者將能夠在未來變更 VM 的安全性設定，而「美國東部資料中心」則可執行 VM，但不會變更其設定。
如果您有一個以上的守護者，請以逗號分隔監護人的名稱，例如 `'EAST-US Datacenter', 'EMEA Datacenter'` 。
磁片區識別碼辨識符號會指定您是否只信任相同版本 (等於) 的範本磁片或未來版本 (GreaterThanOrEquals) 。
磁片名稱和簽署憑證必須完全符合進行部署時所要考慮的版本比較。
您可以藉由提供以逗號分隔的磁片區識別碼限定詞清單來信任多個範本磁片 `-VolumeIDQualifier` 。
最後，如果您有其他需要伴隨回應檔案與 VM 的檔案，請使用 `-OtherFile` 參數並提供以逗號分隔的檔案路徑清單。

請參閱 [ShieldingDataFile](/powershell/module/shieldedvmdatafile/new-shieldingdatafile) 和 [VolumeIDQualifier](/powershell/module/shieldedvmdatafile/New-VolumeIDQualifier) 的 Cmdlet 檔，以瞭解設定防護資料檔案的其他方法。

## <a name="additional-references"></a>其他參考資料

- [部署受防護的 VM](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)

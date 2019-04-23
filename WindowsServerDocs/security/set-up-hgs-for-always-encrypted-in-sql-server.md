---
title: 設定 永遠加密 SQL Server 中的 主機守護者服務
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/03/2018
ms.openlocfilehash: 2f800dfa01077287f8200dd8abea0be899776683
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866689"
---
# <a name="setting-up-the-host-guardian-service-for-always-encrypted-with-secure-enclaves-in-sql-server"></a>設定 Always Encrypted 的使用中 SQL Server 的安全 enclaves 主機守護者服務 

>適用於：Windows Server （半年通道），Windows Server 2019，SQL Server 2019 預覽
 
[一律加密與安全 enclaves](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves)中 SQL Server 2019 預覽是設計來啟用機密計算儲存在資料庫中的敏感性資料的功能。 主機守護者服務 (HGS) 播放安全飛地，設定 Always encrypted 是虛擬化型安全性 (VBS) 記憶體 enclave 時保留您的資料安全的重要角色。 VBS 記憶體 enclave 的安全性取決於 Windows Hypervisor 的安全性和更廣泛地裝載 SQL Server 之電腦的安全性。 

因此，資料庫用戶端應用程式允許使用 Always Encrypted 的機密資料上執行計算的 VBS 記憶體記憶體保護區之前，應用程式必須與受信任的 HGS 證明。 證明證明裝載 SQL Server，其包含 enclave，處於正確狀態，而且可以是受信任的電腦。 本主題的其餘部分，我們會以裝載 SQL Server 作為只是主機電腦的電腦。

有兩個互斥的方式，讓應用程式，證明客戶資料： 

- 主機金鑰證明會授權主機透過證明信任它擁有的已知且受信任的私用金鑰。 主機金鑰證明和實體主機電腦或執行 SQL Server 的第 2 代虛擬機器建議進入生產階段前環境。
- TPM 證明驗證硬體度量的大小，請確認主機執行時，只有正確的二進位檔和安全性原則。 TPM 證明與執行 SQL Server 的實體主機機器 （不是虛擬機器） 被建議用於生產環境中。

如需有關主機守護者服務，它可以測量的詳細資訊，請參閱[受防護網狀架構和受防護的 Vm 概觀](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms)。 請注意，雖然文件是由受防護的 Vm 所討論，相同的保護、 架構和最佳作法適用於 SQL Server 使用 Always Encrypted VBS enclaves。 

本文將協助您在 建議的設定中設定 HGS。 

## <a name="prerequisites"></a>必要條件 

本章節涵蓋 HGS 和主機電腦的必要條件。 

### <a name="hgs-servers"></a>HGS 伺服器

- 若要執行的 HGS 1-3 伺服器。 

  >[!NOTE]
  >只有 1 HGS 伺服器，才能測試或生產階段前環境。

  因為他們控制哪些機器可以執行您的 SQL Server 執行個體，使用 Always Encrypted 與安全 enclaves 這些伺服器應該嚴密地受到保護。 
  建議您使用不同的系統管理員管理 HGS 叢集和您基礎結構，或在個別的虛擬化網狀架構或 Azure 訂用帳戶中的其餘部分隔離的實體硬體上執行的 HGS。

  - Windows Server 2019 Standard 或 Datacenter edition。
  - 2 個 Cpu
  - 8GB RAM
  - 100 GB 的儲存空間

  您可以使用[長期維護通道 (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc)或[半年通道](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel)。 
  若要註冊並下載 Windows Server Insider Preview，請參閱[開始使用 Windows Insider 計劃](https://insider.windows.com/for-business-getting-started-server/)。

- 選擇 主機守護者服務所建立的新 Active Directory 樹系的名稱。 
  HGS 不應加入至您現有的公司網域，並應該有個別的系統管理員進行管理。   

- 防火牆和路由規則以允許從主機守護者服務節點上的輸入的 HTTP (TCP 80) 或 HTTPS (TCP 443) 的流量： 

  - 執行 SQL Server 的電腦
  - 執行資料庫用戶端應用程式 （例如網頁伺服器） 發出資料庫查詢，並使用 Always Encrypted 安全 enclaves 的機器。 

### <a name="sql-server-host-machines"></a>SQL Server 主機電腦

- 您的 SQL Server 執行個體應符合下列需求的電腦上執行：

  - Windows: 
    - Windows 10 企業版，版本 1809  
    - Windows Server 2019 Datacenter （半年通道），版本 1809
    - Windows Server 2019 Datacenter
  - 適用於生產環境或第 2 代虛擬機器進行測試 （請注意，Azure 不支援第 2 代 Vm） 的實體機器
  - 中所列的一般需求[硬體和軟體需求，安裝 SQL server](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017)。   

  您可以使用[長期維護通道 (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc)或[半年通道](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel)。 
  若要註冊並下載 Windows Server Insider Preview，請參閱[開始使用 Windows Insider 計劃](https://insider.windows.com/for-business-getting-started-server/)。

- 所選的證明模式的特定需求：
  - **TPM 模式**是最強的證明模式，而且會使用以密碼編譯方式驗證您的資料中心 （使用來自每個 TPM 的唯一識別碼），執行受信任的硬體和韌體，已知主機電腦的受信任的平台模組 (TPM)組態 （使用 TPM 基準），並執行可信任的核心和使用者模式程式碼 （使用 Windows Defender 應用程式控制）。 下列硬體才能使用 TPM 模式： 
    - TPM 2.0 模組，安裝並啟用 
    - Microsoft 安全開機原則已啟用安全開機 （不使用第 3 個合作對象安全開機的 CA 原則或任何自訂原則啟用）
    - Iommu 所致 （Intel vt-d 或 AMD SR-IOV），以防止直接記憶體存取的攻擊 

  - **主索引鍵模式**用以識別及授權想要執行 SQL Server 的主機電腦的非對稱金鑰組 （很像 SSH 金鑰）。 此模式可以更輕鬆地設定和並沒有任何特定的硬體需求，但不是會驗證軟體或主機機器上執行的韌體。  

若要檢查您的 TPM 是否相容，執行下列命令在主機電腦上您想要用來執行 SQL Server 使用 Always Encrypted 與安全 enclaves。 "2.0"必須出現在清單中，供您使用 TPM 證明的支援 SpecVersions 的：

```powershell
Get-CimInstance -ClassName Win32_Tpm -Namespace root/cimv2/Security/MicrosoftTpm 
```

## <a name="set-up-the-first-hgs-node"></a>設定第一個 HGS 節點 

主機守護者服務的運作使用 3 個節點叢集的高可用性組態。 建議您設定一個節點完全之前新增其他節點。 

在提升權限的 PowerShell 工作階段中執行所有下列命令。

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. [!INCLUDE [Install HGS by default](../../includes/install-hgs-default.md)] 

3. [!INCLUDE [Determine a DNN](../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

   Always encrypted 與安全 enclaves 執行 SQL Server 和機器的主機電腦會執行資料庫用戶端應用程式，兩者都必須向 HGS，但是主機電腦需要證明。

4. 在機器重新開機之後，將會安裝 HGS，而且伺服器也會與設定的 Active Directory 網域控制站。 
   在 Active Directory 組態中，本機電腦系統管理員帳戶加入 Domain Admins 群組中，只有此群組的成員可以登入網域控制站。
   使用網域系統管理員帳戶登入 (例如administrator@bastion.local或 bastion.local\administrator) 或建立另一個網域系統管理員帳戶登入，然後設定 證明服務。
   您必須選擇 TPM 或主機金鑰證明，並執行對應的命令。 
   HgsServiceName，指定您選擇的 DNN。

   TPM 模式：
   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustTpm
   ```

   主索引鍵模式：
   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustHostKey 
   ```

5. 請確定執行 SQL Server 的主機電腦能夠解析 DNS 名稱的新 HGS 網域成員藉由設定 從您公司的 DNS 伺服器的轉寄站，以新的 HGS 網域控制站。 您使用 Windows Server DNS，可以在您的組織中的 DNS 伺服器上，在提升權限的 PowerShell 主控台中執行下列命令來設定條件轉寄站。 以下 Windows PowerShell 語法，視您的環境中的位址和名稱取代。 您新增更多的 HGS 節點之後，執行此命令一次以將它們加入做為主要伺服器。

   ```powershell
   Add-DnsServerConditionalForwarderZone -Name <HGS domain name> -ReplicationScope "Forest" -MasterServers <IP addresses of HGS servers>
   ```

## <a name="set-up-additional-hgs-nodes-for-production-deployments"></a>設定生產環境部署的其他 HGS 節點

若要新增節點到叢集，請在提升權限的 PowerShell 工作階段中執行下列命令。 

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. 設定 DNS 用戶端解析程式，以指向您的主要 HGS 伺服器，以便它可以解決您的 HGS 網域名稱。 如果您使用桌面體驗的伺服器，您可以在 [網路和共用中心]。 您可以使用 Server Core **sconfig.exe**工具，選項 8，或**組 DnsClientServerAddress**設定的 DNS 位址。 

3. 接下來，我們將會升級此伺服器為網域控制站。 您將需要網域系統管理員認證才能完成這項工作，並會提示您輸入目錄服務修復模式密碼，執行命令之後。 

   ```powershell
   $HgsDomainName = 'bastion.local' 
   $HgsDomainCredential = Get-Credential 
 
   Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $HgsDomainCredential -Restart 
   ```

4. [!INCLUDE [Initialize HGS](../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="configure-hgs-for-https"></a>針對 HTTPS 設定 HGS 

根據預設，當您初始化 HGS 伺服器時它會設定 IIS 網站，僅限 HTTP 通訊。

>[!NOTE]
>設定 HTTPS，並使用已知且受信任的 HGS 伺服器憑證，才能防止攔截攻擊，因此建議用於生產環境部署。

[!INCLUDE [Configure HTTPS](../../includes/configure-hgs-for-https.md)] 

>[!NOTE]
>Always encrypted 與安全 enclaves 的 SSL 憑證必須在兩部執行 SQL Server 的主機機器上受到信任，並執行資料庫用戶端應用程式的機器必須向 HGS。 

## <a name="collect-attestation-info-from-the-host-machines"></a>從主機電腦收集證明資訊

HGS 已設定完畢後，它必須設定證明資訊從主機電腦，讓它知道哪些機器應該要授權以執行使用 Always Encrypted 和 VBS 安全 enclaves 機密的計算。 這些步驟視您使用的證明模式。 

### <a name="collect-tpm-attestation-artifacts"></a>收集 TPM 證明成品 

如果您使用 TPM 模式，請在提升權限的 PowerShell 工作階段中執行下列命令，安裝支援的證明和收集的資訊，您必須向主機守護者服務的每個主機電腦上。 

1. 若要安裝 HGS 用戶端主機電腦上，安裝也會安裝 HYPER-V 的受防護主機功能。 
   當您將無法在這部電腦上執行的 Vm 時，hypervisor，才能啟用隔離 VBS enclaves 虛擬化型安全性功能。

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. 重新啟動您的電腦，當系統提示您完成安裝 Hyper-v 的安全。 
3. 撰寫的程式碼完整性原則，以限制哪些軟體可在系統上執行。 
   任何 Windows Defender 應用程式控制原則就已足夠。 
   如果您只會在伺服器上執行 Microsoft 軟體，下列命令會快速地為您建立原則。 
   原則將會在稽核模式中，這表示它將會記錄關於未經授權的程式碼的事件，但會保留它的執行。  

   ```powershell
   mkdir C:\artifacts
   Copy-Item C:\Windows\Schemas\CodeIntegrity\ExamplePolicies\AllowMicrosoft.xml C:\artifacts\AllowMicrosoft-Audit.xml 
   Set-RuleOption -FilePath C:\artifacts\AllowMicrosoft-Audit.xml -Option 3 
   ConvertFrom-CIPolicy -XmlFilePath C:\artifacts\AllowMicrosoft-Audit.xml -BinaryFilePath C:\artifacts\AllowMicrosoft-Audit.bin 
   Copy-Item C:\artifacts\AllowMicrosoft-Audit.bin C:\Windows\System32\CodeIntegrity\SIPolicy.p7b 
   Restart-Computer 
   ```

   Windows Defender 應用程式控制會有許多的功能，以涵蓋各種不同的安全性 postures。 
   如果您要允許非 Microsoft 軟體或自訂的預設原則，se [Windows Defender 應用程式控制部署指南](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide)。   


4. 確認虛擬化型安全性會使用下列命令在電腦上執行。 
   您會知道 VBS 正在 DeviceGuardSecurityServicesRunning 欄位是否列在它 「 HypervisorEnforcedCodeIntegrity"。 
   如果未執行，下載[裝置防護 Readiness Tool](https://www.microsoft.com/download/details.aspx?id=53337)並執行"DG_Readiness.ps1-啟用-hvci 原則 」 來啟用它。  
   
   ```powershell
   Get-ComputerInfo -Property DeviceGuard* 
   ```
5. 收集的 TPM 識別碼和基準：

   ```powershell 
   (Get-PlatformIdentifier -Name $env:computername).Save("C:\artifacts\TpmID-$env:computername.xml") 
   Get-HgsAttestationBaselinePolicy -SkipValidation -Path "C:\artifacts\TpmBaseline-$env:computername.tcglog" 
   ```
   
6. 將 xml、 tcglog 和 bin 檔案從 C:\artifacts 複製到您的 HGS 伺服器。
7. 如果這是您要新增到 HGS 伺服器第一部 TPM 主機時，您必須在每一部 HGS 伺服器上安裝受信任的 TPM 根憑證。 
   請遵循[HGS 文件的指引](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-install-trusted-tpm-root-certificates)才能完成此步驟。
8. HGS 伺服器授權通過證明，這台主機。 
   下列指令碼會假設 3 個檔案複製到 C:\temp HGS 伺服器上，且您的伺服器電腦名稱是"ServerA"。 
   調整的路徑和名稱，以符合您自己的環境。 

   ```powershell
   Add-HgsAttestationTpmHost -Name ServerA -Path C:\temp\TpmID-ServerA.xml 
   Add-HgsAttestationTpmPolicy -Name ServerA-Baseline -Path C:\temp\TpmBaseline-ServerA.tcglog 
   Add-HgsAttestationCiPolicy -Name AllowMicrosoft-Audit -Path C:\temp\AllowMicrosoft-Audit.bin 
   ```
9. 您第一部伺服器現在已經準備好，證明客戶資料的 ！ 
   在主機上，執行下列命令，以告知它 （您的 HGS 叢集的 DNS 名稱，您通常會使用與 HGS 網域名稱結合的 HGS 服務名稱的變更），證明客戶資料的位置。 
   如果您收到 HostUnreachable 錯誤，請確定您可以解析，並 ping 您的 HGS 伺服器的 DNS 名稱。 

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl https://hgs.bastion.local/Attestation -KeyProtectionServerUrl https://hgs.bastion.local/KeyProtection/ 
   ```

10. 上述命令的結果應該會顯示該 AttestationStatus = 成功。 如果不存在，請參閱[證明失敗](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hosts#attestation-failures)如需如何解決此錯誤的指引。   
11. 針對每個主機電腦重複步驟 1-10。 
    如果您使用完全相同的硬體，您不需要擷取新的基準或每部電腦的 CI 原則。 
    從您的第一部伺服器的基準設定相同的所有機器，以及 CI 原則可重複使用多部電腦上，只要 Microsoft 軟體是在電腦上唯一的軟體。

### <a name="collecting-host-keys"></a>收集主機金鑰 

>[!NOTE] 
>主機金鑰證明只建議在測試環境中使用。 TPM 證明提供最強的保證 VBS enclaves 處理您的機密資料，SQL Server 上執行受信任的程式碼，以及機器設定建議的安全性設定。 

如果您選擇在 主機金鑰證明模式中設定 HGS，表示您要產生並收集每個主機電腦上的索引鍵和向主機守護者服務。 

1. 若要取得 HGS 用戶端安裝在主機電腦上，安裝也會安裝 HYPER-V 的受防護主機功能。 
   當您將無法在這部電腦上執行的 Vm 時，hypervisor，才能啟用隔離執行 Always Encrypted 的查詢 VBS enclaves 虛擬化型安全性功能。 

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. 重新啟動您的電腦，當系統提示您完成安裝 Hyper-v 的安全。
3. 產生唯一的主機金鑰，並將產生的公開金鑰匯出至檔案。 

   ```powershell
   Set-HgsClientHostKey 
   mkdir C:\artifacts 
   Get-HgsClientHostKey -Path C:\artifacts\$env:computername.cer 
   ```
   
   或者，您可以指定憑證指紋，如果您想要使用您自己的憑證。 
   這可以是很有用，如果您想要共用憑證跨多部電腦，或使用憑證，繫結到 TPM 或 HSM。 建立 TPM 繫結憑證 （這可防止它擁有的私用金鑰被偷，並且在另一部電腦上使用，而且需要僅 TPM 1.2） 的範例如下：

   ```powershell
   $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
   Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
   ```

4. 主機將金鑰複製到 「 主機守護者服務。 
5. 向任何 HGS 節點中，為您的環境中使用相關的名稱和路徑中的主機金鑰： 

   ```powershell
   Add-HgsAttestationHostKey -Name 'ServerA' -Path C:\temp\ServerA.cer 
   ``` 

6. 您第一部伺服器現在已經準備好，證明客戶資料的 ！ 
   在主機上，執行下列命令，以告知它 （您的 HGS 叢集的 DNS 名稱，您通常會使用與 HGS 網域名稱結合的 HGS 服務名稱的變更），證明客戶資料的位置。 
   如果您收到 HostUnreachable 錯誤，請確定您可以解析，並 ping 您的 HGS 伺服器的 DNS 名稱。    

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl http://hgs.bastion.local/Attestation -KeyProtectionServerUrl http://hgs.bastion.local/KeyProtection/  
   ```

7. 上述命令的結果應該會顯示該 AttestationStatus = 成功。 
   如果您收到 HostUnreachable 錯誤，就表示您的主機電腦無法與 HGS 通訊。 
   請確定 DNS 解析已設定好，主機與 HGS 伺服器之間，您可以 ping 伺服器。 
   UnauthorizedHost 錯誤指出與 HGS 伺服器 – 重複步驟 4 和 5，以解決此錯誤未註冊的公開金鑰。 
   如果所有解決方案均失敗，請執行清除 HgsClientHostKey，並重複步驟 3 到 6。   

8. 針對將執行 SQL Server 使用 Always Encrypted VBS enclaves 每部伺服器重複步驟 1-7。     



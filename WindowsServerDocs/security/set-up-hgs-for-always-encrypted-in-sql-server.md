---
title: 在 SQL Server 中設定 Always Encrypted 的主機守護者服務
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/03/2018
ms.openlocfilehash: 74146e854f4183970d2b92bb26babbd1b31f0bc2
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870349"
---
# <a name="setting-up-the-host-guardian-service-for-always-encrypted-with-secure-enclaves-in-sql-server"></a>在 SQL Server 中使用安全記憶體保護區設定 Always Encrypted 的主機守護者服務 

>適用於：Windows Server （半年通道）、Windows Server 2019、SQL Server 2019 preview
 
SQL Server 2019 preview 中[的安全記憶體保護區 Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves)是一項功能，其設計目的是針對儲存在資料庫中的敏感性資料啟用機密計算。 主機守護者服務（HGS）在安全記憶體保護區（設定為 Always Encrypted）是以虛擬化為基礎的安全性（VBS）記憶體記憶體保護區時，扮演重要的角色來確保您的資料安全。 VBS 記憶體記憶體保護區的安全性取決於 Windows 虛擬機器的安全性，以及更廣泛的 SQL Server 裝載的電腦安全性性。 

因此，在資料庫用戶端應用程式允許 Always Encrypted 用來執行敏感性資料計算的 VBS 記憶體記憶體保護區之前，應用程式必須使用受信任的 HGS 進行證明。 證明會證明主控 SQL Server 的機器，其中包含記憶體保護區、處於正確狀態，而且可以受到信任。 在本主題的其餘部分，我們會將裝載 SQL Server 的電腦稱為「主機電腦」。

應用程式有兩個互斥的方式可證明： 

- 主機金鑰證明會藉由證明主機擁有已知且受信任的私密金鑰來授權主機。 針對預先生產環境，建議使用主機金鑰證明，以及執行 SQL Server 的實體主機電腦或第2代虛擬機器。
- TPM 證明會驗證硬體測量，以確保主機僅執行正確的二進位檔和安全性原則。 建議您在生產環境中執行 TPM 證明和執行 SQL Server 的實體主機電腦（非虛擬機器）。

如需主機守護者服務及其可測量之內容的詳細資訊，請參閱[受防護的網狀架構與受防護的 vm 總覽](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms)。 請注意，雖然檔會討論受防護的 Vm，但相同的保護、架構和最佳作法適用于使用 VBS 記憶體保護區的 SQL Server Always Encrypted。 

本文將協助您在建議的設定中設定 HGS。 

## <a name="prerequisites"></a>必要條件 

本節涵蓋 HGS 和主機電腦的必要條件。 

### <a name="hgs-servers"></a>HGS 伺服器

- 要執行 HGS 的1-3 伺服器。 

  > [!NOTE]
  > 測試或預先生產環境只需要1個 HGS 伺服器。

  這些伺服器應謹慎地受到保護，因為它們會控制哪些電腦可以使用具有安全記憶體保護區的 Always Encrypted 來執行您的 SQL Server 實例。 
  建議不同的系統管理員管理 HGS 叢集，並在實體硬體上執行 HGS 與基礎結構的其餘部分隔離，或在個別的虛擬化網狀架構或 Azure 訂用帳戶中執行。

  - Windows Server 2019 Standard 或 Datacenter edition。
  - 2個 Cpu
  - 8GB RAM
  - 100GB 儲存體

  您可以使用[長期維護通道（LTSC）](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc)或[半年通道](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel)。 
  若要註冊並下載 Windows Server Insider Preview，請參閱[開始使用 Windows 測試人員程式](https://insider.windows.com/for-business-getting-started-server/)。

- 為主機守護者服務所建立的新 Active Directory 樹系選擇名稱。 
  HGS 不應加入您現有的公司網域，而且應該有個別的系統管理員來管理它。   

- 防火牆和路由規則，以允許主機守護者服務節點上的輸入 HTTP （TCP 80）或 HTTPS （TCP 443）流量： 

  - 執行 SQL Server 的電腦
  - 執行資料庫用戶端應用程式（例如 web 伺服器）的電腦，其會發出資料庫查詢，並使用 Always Encrypted 安全記憶體保護區。 

### <a name="sql-server-host-machines"></a>SQL Server 主機電腦

- 您的 SQL Server 實例應該在符合下列需求的電腦上執行：

  - 時段 
    - Windows 10 企業版（版本1809）  
    - Windows Server 2019 Datacenter （半年通道），版本1809
    - Windows Server 2019 Datacenter
  - 用於生產的實體機器，或用於測試的第2代虛擬機器（請注意，Azure 不支援第2代 Vm）
  - [安裝 SQL Server 的硬體和軟體需求](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017)中列出的一般需求。   

  您可以使用[長期維護通道（LTSC）](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc)或[半年通道](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel)。 
  若要註冊並下載 Windows Server Insider Preview，請參閱[開始使用 Windows 測試人員程式](https://insider.windows.com/for-business-getting-started-server/)。

- 所選證明模式的特定需求：
  - **TPM 模式**是最強的證明模式，會使用可信賴平臺模組（tpm）來以密碼編譯方式驗證您的主機電腦是否已知道您的資料中心（使用每個 TPM 的唯一識別碼），並執行受信任的硬體和固件設定（使用 TPM 基準），以及執行可信任的內核和使用者模式程式碼（使用 Windows Defender 應用程式控制）。 需要下列硬體才能使用 TPM 模式： 
    - 已安裝並啟用 TPM 2.0 模組 
    - 已使用 Microsoft 安全開機原則啟用安全開機（請勿啟用協力廠商安全開機 CA 原則或任何自訂原則）
    - IOMMU （Intel VT-d 或 AMD SR-IOV）以防止直接記憶體存取攻擊 

  - **主機金鑰模式**會使用非對稱金鑰組（非常類似 SSH 金鑰）來識別和授權想要執行 SQL Server 的主機電腦。 此模式較容易設定，而且沒有任何特定的硬體需求，但不會驗證在主機電腦上執行的軟體或固件。  

若要檢查您的 TPM 是否相容，請在您想要使用具有安全記憶體保護區的 Always Encrypted 執行 SQL Server 的主機電腦上執行下列命令。 「2.0」必須出現在支援的 SpecVersions 清單中，您才能使用 TPM 證明：

```powershell
Get-CimInstance -ClassName Win32_Tpm -Namespace root/cimv2/Security/MicrosoftTpm 
```

## <a name="set-up-the-first-hgs-node"></a>設定第一個 HGS 節點 

主機守護者服務會使用3個節點的叢集，在高可用性設定中運作。 建議您先完整設定一個節點，再新增其他節點。 

在提高許可權的 PowerShell 會話中執行下列所有命令。

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. [!INCLUDE [Install HGS by default](../../includes/install-hgs-default.md)] 

3. [!INCLUDE [Determine a DNN](../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

   對於具有安全記憶體保護區的 Always Encrypted，執行 SQL Server 的主機電腦和執行資料庫用戶端應用程式的電腦都需要連線 HGS，但只有主機電腦需要證明。

4. 電腦重新開機後，將會安裝 HGS，而且伺服器也會是已設定 Active Directory 的網域控制站。 
   在 Active Directory 設定期間，會將本機電腦系統管理員帳戶新增至 Domain Admins 群組，而且只有此群組的成員可以登入網域控制站。
   使用網域系統管理員帳戶（ administrator@bastion.local例如或 local\administrator）登入，或建立另一個網域管理帳戶以進行登入，然後設定證明服務。
   您將需要選擇 [TPM] 或 [主機金鑰證明]，然後執行對應的命令。 
   針對 [HgsServiceName]，指定您選擇的 DNN。

   若為 TPM 模式：

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustTpm
   ```

   針對 [主機金鑰模式]：

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustHostKey 
   ```

5. 請確定執行 SQL Server 的主機電腦能夠將您的公司 DNS 伺服器轉寄站，設定為新的 HGS 網域控制站，以解析新 HGS 網域成員的 DNS 名稱。 如果您使用的是 Windows Server DNS，您可以在組織中 DNS 伺服器上已提升許可權的 PowerShell 主控台中執行下列命令，以設定條件轉寄站。 視您環境的需要，替代下列 Windows PowerShell 語法中的名稱和位址。 新增更多 HGS 節點之後，請再次執行此命令，將它們新增為主伺服器。

   ```powershell
   Add-DnsServerConditionalForwarderZone -Name <HGS domain name> -ReplicationScope "Forest" -MasterServers <IP addresses of HGS servers>
   ```

## <a name="set-up-additional-hgs-nodes-for-production-deployments"></a>針對生產環境部署設定額外的 HGS 節點

若要將節點新增至叢集，請在提升許可權的 PowerShell 會話中執行下列命令。 

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. 將 DNS 用戶端解析程式設定為指向您的主要 HGS 伺服器，讓它可以解析您的 HGS 功能變數名稱。 如果您使用 [具有桌面體驗的伺服器]，則可以在 [網路和共用中心] 執行此動作。 在 Server Core 上，您可以使用**sconfig**工具、選項8或**DnsClientServerAddress**設定 DNS 位址。 

3. 接下來，我們會將此伺服器升級為網域控制站。 您將需要網域系統管理員認證才能完成這項工作，並會在執行命令之後提示您輸入目錄服務修復模式的密碼。 

   ```powershell
   $HgsDomainName = 'bastion.local' 
   $HgsDomainCredential = Get-Credential 
 
   Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $HgsDomainCredential -Restart 
   ```

4. [!INCLUDE [Initialize HGS](../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="configure-hgs-for-https"></a>設定 HGS 以進行 HTTPS 

根據預設，當您初始化 HGS 伺服器時，它會將 IIS 網站設定為僅限 HTTP 通訊。

> [!NOTE]
> 需要使用已知且受信任的 HGS 伺服器憑證來設定 HTTPS，以防止攔截式攻擊，因此建議用於生產環境部署。

[!INCLUDE [Configure HTTPS](../../includes/configure-hgs-for-https.md)] 

> [!NOTE]
> 對於具有安全記憶體保護區的 Always Encrypted，必須在執行 SQL Server 的主機電腦上信任 SSL 憑證，而且執行資料庫用戶端應用程式的電腦必須與 HGS 連線。 

## <a name="collect-attestation-info-from-the-host-machines"></a>從主機電腦收集證明信息

設定 HGS 之後，必須使用您主機電腦的證明信息來設定它，讓它知道哪些機器應該獲得授權，以使用 Always Encrypted 和 VBS 安全記憶體保護區來執行機密計算。 這些步驟會根據您使用的證明模式而有所不同。 

### <a name="collect-tpm-attestation-artifacts"></a>收集 TPM 證明成品 

如果您使用 TPM 模式，請在每部主機電腦上提高許可權的 PowerShell 會話中執行下列命令，以安裝證明的支援，並收集您需要向主機守護者服務註冊的資訊。 

1. 若要在您的主機電腦上安裝 HGS 用戶端，請安裝受防護主機功能，這也會安裝 Hyper-v。 
   雖然您不會在這部電腦上執行 Vm，但必須使用虛擬程式來啟用隔離 VBS 記憶體保護區的虛擬化安全性功能。

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. 當系統提示您完成 Hyper-v 的安裝時，請重新開機電腦。 
3. 撰寫程式碼完整性原則，以限制可以在系統上執行的軟體。 
   任何 Windows Defender 應用程式控制原則都已足夠。 
   如果您只是在伺服器上執行 Microsoft 軟體，下列命令將會為您快速建立原則。 
   原則會處於 audit 模式，這表示它會記錄有關未經授權程式碼的事件，但不會讓它無法執行。  

   ```powershell
   mkdir C:\artifacts
   Copy-Item C:\Windows\Schemas\CodeIntegrity\ExamplePolicies\AllowMicrosoft.xml C:\artifacts\AllowMicrosoft-Audit.xml 
   Set-RuleOption -FilePath C:\artifacts\AllowMicrosoft-Audit.xml -Option 3 
   ConvertFrom-CIPolicy -XmlFilePath C:\artifacts\AllowMicrosoft-Audit.xml -BinaryFilePath C:\artifacts\AllowMicrosoft-Audit.bin 
   Copy-Item C:\artifacts\AllowMicrosoft-Audit.bin C:\Windows\System32\CodeIntegrity\SIPolicy.p7b 
   Restart-Computer 
   ```

   Windows Defender 應用程式控制有許多功能可涵蓋各種不同的安全性 postures。 
   如果您需要允許非 Microsoft 軟體或自訂預設原則，請進行[Windows Defender 應用程式控制部署指南](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide)。   


4. 使用下列命令確認電腦上的虛擬化安全性正在執行。 
   如果 [DeviceGuardSecurityServicesRunning] 欄位中列出了「HypervisorEnforcedCodeIntegrity」，則您會知道 VBS 正在執行。 
   如果未執行，請下載[Device Guard 準備就緒工具](https://www.microsoft.com/download/details.aspx?id=53337)並執行 "DG_READINESS-HVCI" 以啟用它。  
   
   ```powershell
   Get-ComputerInfo -Property DeviceGuard* 
   ```

5. 收集 TPM 識別碼和基準：

   ```powershell 
   (Get-PlatformIdentifier -Name $env:computername).Save("C:\artifacts\TpmID-$env:computername.xml") 
   Get-HgsAttestationBaselinePolicy -SkipValidation -Path "C:\artifacts\TpmBaseline-$env:computername.tcglog" 
   ```
   
6. 將 xml、tcglog 和 bin 檔案從 C:\artifacts 複製到您的 HGS 伺服器。
7. 如果這是您要新增至 HGS 伺服器的第一個 TPM 主機，您必須在每部 HGS 伺服器上安裝受信任的 TPM 根憑證。 
   遵循[HGS 檔上的指導](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-install-trusted-tpm-root-certificates)方針，完成此步驟。
8. 在 HGS 伺服器上，授權此主機通過證明。 
   下列腳本假設3個檔案已複製到 HGS 伺服器上的 C：\temp，而您的伺服器電腦名稱稱是 "ServerA"。 
   調整路徑和名稱，使其符合您自己的環境。 

   ```powershell
   Add-HgsAttestationTpmHost -Name ServerA -Path C:\temp\TpmID-ServerA.xml 
   Add-HgsAttestationTpmPolicy -Name ServerA-Baseline -Path C:\temp\TpmBaseline-ServerA.tcglog 
   Add-HgsAttestationCiPolicy -Name AllowMicrosoft-Audit -Path C:\temp\AllowMicrosoft-Audit.bin 
   ```

9. 您的第一部伺服器現在已經準備好證明了！ 
   在主機電腦上執行下列命令，告訴它要證明的位置（將 DNS 名稱變更為您的 HGS 叢集，通常您會使用與 HGS 功能變數名稱結合的 HGS 服務名稱）。 
   如果您收到 HostUnreachable 錯誤，請確定您可以解析和 ping HGS 伺服器的 DNS 名稱。 

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl https://hgs.bastion.local/Attestation -KeyProtectionServerUrl https://hgs.bastion.local/KeyProtection/ 
   ```

10. 上述命令的結果應該會顯示 AttestationStatus = 已通過。 如果沒有，請參閱[證明失敗](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hosts#attestation-failures)，以取得如何解決錯誤的指引。   
11. 針對每部主機電腦重複步驟1-10。 
    如果您使用相同的硬體，就不需要為每部機器捕獲新的基準或 CI 原則。 
    您的第一部伺服器的基準將涵蓋所有設定完全相同的機器，而且只要 Microsoft 軟體是電腦上唯一的軟體，CI 原則就可以跨多部電腦重複使用。

### <a name="collecting-host-keys"></a>收集主機金鑰 

> [!NOTE] 
> 只有在測試環境中才建議使用主機金鑰證明。 TPM 證明提供 VBS 記憶體保護區處理敏感性資料的最強保證，SQL Server 正在執行受信任的程式碼，而且電腦會以建議的安全性設定進行設定。 

如果您選擇在主機金鑰證明模式中設定 HGS，您必須從每部主機電腦產生並收集金鑰，並向主機守護者服務註冊。 

1. 若要讓 HGS 用戶端安裝在您的主機電腦上，請安裝受防護主機功能，這也會安裝 Hyper-v。 
   雖然您不會在這部電腦上執行 Vm，但必須要有虛擬機器，才能啟用隔離 Always Encrypted 查詢的 VBS 記憶體保護區的虛擬化安全性功能。 

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. 當系統提示您完成 Hyper-v 的安裝時，請重新開機電腦。
3. 產生唯一的主機金鑰，並將產生的公開金鑰匯出至檔案。 

   ```powershell
   Set-HgsClientHostKey 
   mkdir C:\artifacts 
   Get-HgsClientHostKey -Path C:\artifacts\$env:computername.cer 
   ```
   
   或者，如果您想要使用自己的憑證，可以指定指紋。 
   如果您想要跨多部電腦共用憑證，或使用系結至 TPM 或 HSM 的憑證，這會很有用。 以下是建立 TPM 系結憑證的範例（這會使私密金鑰無法被盜用，並在另一部電腦上使用，而且只需要 TPM 1.2）：

   ```powershell
   $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
   Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
   ```

4. 將主機金鑰複製到主機守護者服務。 
5. 使用您環境的相關名稱和路徑，向任何 HGS 節點註冊主機金鑰： 

   ```powershell
   Add-HgsAttestationHostKey -Name 'ServerA' -Path C:\temp\ServerA.cer 
   ``` 

6. 您的第一部伺服器現在已經準備好證明了！ 
   在主機電腦上執行下列命令，告訴它要證明的位置（將 DNS 名稱變更為您的 HGS 叢集，通常您會使用與 HGS 功能變數名稱結合的 HGS 服務名稱）。 
   如果您收到 HostUnreachable 錯誤，請確定您可以解析和 ping HGS 伺服器的 DNS 名稱。    

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl http://hgs.bastion.local/Attestation -KeyProtectionServerUrl http://hgs.bastion.local/KeyProtection/  
   ```

7. 上述命令的結果應該會顯示 AttestationStatus = 已通過。 
   如果您收到 HostUnreachable 錯誤，這表示您的主機電腦無法與 HGS 通訊。 
   確定主機電腦與 HGS 伺服器之間已設定 DNS 解析，而且您可以 ping 伺服器。 
   UnauthorizedHost 錯誤指出公開金鑰未向 HGS 伺服器註冊-重複步驟4和5以解決錯誤。 
   如果其他所有動作都失敗，請執行清除 HgsClientHostKey 並重複步驟3-6。   

8. 針對將使用 VBS 記憶體保護區 Always Encrypted 執行 SQL Server 的每部伺服器，重複步驟1-7。     



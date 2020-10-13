---
title: 疑難排解主機守護者服務
ms.topic: article
ms.assetid: 424b8090-0692-49a6-9dc4-3c0e77d74b80
manager: dongill
author: rpsqrd
description: 部署或操作受防護網狀架構中的主機守護者服務 (HGS) 伺服器時所遇到的常見問題解決方案。
ms.author: ryanpu
ms.date: 10/12/2020
ms.openlocfilehash: 398029ed048ac835d23ffebb954baad13970b538
ms.sourcegitcommit: c56e74743e5ad24b28ae81668668113d598047c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2020
ms.locfileid: "91987292"
---
# <a name="troubleshooting-the-host-guardian-service"></a>疑難排解主機守護者服務

> 適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

本文說明在受防護網狀架構 (HGS) server 中部署或操作主機守護者服務時所遇到的常見問題解決方案。
如果您不確定問題的本質，請先嘗試在 HGS 伺服器和 Hyper-v 主機上執行受防護網狀 [架構診斷](guarded-fabric-troubleshoot-diagnostics.md) ，以縮小可能的原因。

## <a name="certificates"></a>憑證

HGS 需要數個憑證才能運作，包括系統管理員設定的加密和簽署憑證，以及由 HGS 本身管理的證明憑證。
如果這些憑證設定不正確，HGS 將無法從想要證明或解除鎖定受防護 Vm 的金鑰保護裝置的 Hyper-v 主機提供要求。
下列各節涵蓋與 HGS 設定的憑證相關的常見問題。

### <a name="certificate-permissions"></a>憑證許可權

HGS 必須能夠存取加密和簽署憑證的公開金鑰和私密金鑰（由憑證指紋新增至 HGS）。
具體而言，執行 HGS 服務 (gMSA) 的群組受管理的服務帳戶需要存取金鑰。
若要尋找 HGS 使用的 gMSA，請在 HGS 伺服器上提高許可權的 PowerShell 提示字元中執行下列命令：

```powershell
(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName
```

您授與 gMSA 帳戶存取權以使用私密金鑰的方式取決於金鑰的儲存位置：在電腦上作為本機憑證檔案、在硬體安全模組上 (HSM) ，或使用自訂的協力廠商金鑰儲存提供者。

#### <a name="grant-access-to-software-backed-private-keys"></a>授與軟體支援的私密金鑰存取權

如果您使用的是自我簽署憑證或憑證授權單位單位所發行的憑證，但該憑證 **並非** 儲存在硬體安全性模組或自訂金鑰儲存提供者中，您可以執行下列步驟來變更私密金鑰許可權：

1. 開啟本機憑證管理員 (certlm.msc services.msc) 
2. 展開 [ **個人 > 憑證** ]，然後尋找您想要更新的簽署或加密憑證。
3. 以滑鼠右鍵按一下憑證，然後選取 [所有工作] **> 管理私密金鑰**]。
4. 選取 **[新增]** 以授與新的使用者對憑證私密金鑰的存取權。
5. 在 [物件選擇器] 中，輸入先前找到的 HGS 的 gMSA 帳戶名稱，然後選取 **[確定]**。
6. 確定 gMSA 具有憑證的 **讀取** 許可權。
7. 選取 **[確定** ] 以關閉 [許可權] 視窗。

如果您是在 Server Core 上執行 HGS 或從遠端管理伺服器，您將無法使用本機憑證管理員來管理私密金鑰。
相反地，您將需要下載受防護網狀 [架構工具 powershell 模組](https://www.powershellgallery.com/packages/GuardedFabricTools) ，讓您能夠在 powershell 中管理許可權。

1. 在 Server Core 機器上開啟已提高許可權的 PowerShell 主控台，或使用 PowerShell 遠端處理，並在 HGS 上具有本機系統管理員許可權的帳戶。
2. 執行下列命令以安裝受防護網狀架構工具 PowerShell 模組，並授與 gMSA 帳戶存取私密金鑰的許可權。

```powershell
$certificateThumbprint = '<ENTER CERTIFICATE THUMBPRINT HERE>'

# Install the Guarded Fabric Tools module, if necessary
Install-Module -Name GuardedFabricTools -Repository PSGallery

# Import the module into the current session
Import-Module -Name GuardedFabricTools

# Get the certificate object
$cert = Get-Item "Cert:\LocalMachine\My\$certificateThumbprint"

# Get the gMSA account name
$gMSA = (Get-IISAppPool -Name KeyProtection).ProcessModel.UserName

# Grant the gMSA read access to the certificate
$cert.Acl = $cert.Acl | Add-AccessRule $gMSA Read Allow
```

#### <a name="grant-access-to-hsm-or-custom-provider-backed-private-keys"></a>授與 HSM 或自訂提供者支援之私密金鑰的存取權

如果您的憑證私密金鑰是由硬體安全模組 (HSM) 或自訂金鑰儲存提供者 (KSP) 所支援，則許可權模型將取決於您的特定軟體廠商。
為了獲得最佳結果，請參閱廠商的檔或支援網站，以取得如何針對您的特定裝置/軟體處理私密金鑰許可權的相關資訊。
在所有情況下，HGS 使用的 gMSA 都需要加密、簽署和通訊憑證私密金鑰的 *讀取* 許可權，才能執行簽章和加密作業。

某些硬體安全性模組不支援將私密金鑰的存取權授與特定使用者帳戶;相反地，它們會允許電腦帳戶存取特定金鑰集中的所有金鑰。
對於這類裝置而言，通常就足以讓電腦存取您的金鑰，而 HGS 將能夠利用該連線。

**Hsm 的秘訣**

以下是建議的設定選項，可協助您根據 Microsoft 及其合作夥伴經驗，順利搭配 HGS 使用 HSM 支援的金鑰。
為了方便起見，我們提供了這些秘訣，而且在閱讀時並不保證會正確，也不是由 HSM 製造商背書。
如果您有進一步的問題，請洽詢您的 HSM 製造商，以取得與特定裝置相關的精確資訊。

HSM 品牌/系列      | 建議
----------------------|-------------
Gemalto SafeNet       | 確認憑證要求檔案中的 [金鑰使用方法] 屬性設定為 [0xa0]，讓憑證可用於簽署和加密。 此外，您必須使用本機憑證管理員工具將私密金鑰的 *讀取* 許可權授與 gMSA 帳戶 (查看上述步驟) 。
nCipher nShield        | 確定每個 HGS 節點都能存取包含簽署和加密金鑰的安全世界。 您可能還需要使用本機憑證管理員，將私密金鑰的 *讀取* 許可權授與 gMSA， (查看上述的步驟) 。
Utimaco CryptoServers | 確認憑證要求檔案中的 [金鑰使用方法] 屬性設定為 [0x13]，讓憑證可用於加密、解密和簽署。

### <a name="certificate-requests"></a>憑證要求

如果您在 (PKI) 環境的公開金鑰基礎結構中，使用憑證授權單位單位來發出憑證，您將需要確保您的憑證要求包含 HGS 使用這些金鑰的最低需求。

**簽署憑證**

CSR 屬性 | 必要值
-------------|---------------
演算法    | RSA
金鑰大小     | 至少2048位
金鑰使用方式    | Signature/Sign/DigitalSignature

**加密憑證**

CSR 屬性 | 必要值
-------------|---------------
演算法    | RSA
金鑰大小     | 至少2048位
金鑰使用方式    | 加密/加密/DataEncipherment

**Active Directory 憑證服務範本**

如果您使用 Active Directory 憑證服務 (ADC) 憑證範本來建立憑證，建議您將範本與下列設定搭配使用：

ADC 範本屬性 | 必要值
-----------------------|---------------
提供者類別      | 金鑰儲存提供者
演算法名稱         | RSA
最小金鑰大小       | 2048
目的                | 簽名和加密
金鑰使用延伸模組    | 數位簽章、金鑰加密、資料加密 ( 「允許加密使用者資料」 ) 


### <a name="time-drift"></a>時間漂移

如果您的伺服器時間與其他 HGS 節點或受防護網狀架構中的 Hyper-v 主機有明顯的漂移，您可能會遇到證明簽署者憑證有效性的問題。
證明簽署者憑證是在 HGS 的幕後建立和更新的，可用來簽署證明服務發給受防護主機的健康情況憑證。

若要重新整理證明簽署者憑證，請在提高許可權的 PowerShell 提示字元中執行下列命令。

```powershell
Start-ScheduledTask -TaskPath \Microsoft\Windows\HGSServer -TaskName
AttestationSignerCertRenewalTask
```

或者，您可以開啟 **工作排程器** () taskschd.msc，並流覽至 **工作排程器程式庫 > Microsoft > Windows > HGSServer** 並執行名為 **AttestationSignerCertRenewalTask**的工作，以手動執行排定的工作。

## <a name="switching-attestation-modes"></a>切換證明模式

如果您使用 [HgsServer 指令程式](https://technet.microsoft.com/library/mt652180.aspx) 將 HGS 從 TPM 模式切換為 Active Directory 模式或反之亦然，則 hgs 叢集中的每個節點最多可能需要10分鐘的時間，才會開始強制執行新的證明模式。
這是正常行為。
除非您已使用新的證明模式確認所有主機的證明成功，否則建議您不要移除任何允許主機在先前證明模式下的原則。

**從 TPM 切換至 AD 模式時的已知問題**

如果您在 TPM 模式中初始化 HGS 叢集，並于稍後切換至 Active Directory 模式，則會有已知的問題，導致 HGS 叢集中的其他節點無法切換至新的證明模式。
為確保所有 HGS 伺服器都強制執行正確的證明模式，請 `Set-HgsServer -TrustActiveDirectory` 在 hgs 叢集的 **每個節點上** 執行。
如果您要從 TPM 模式切換至 AD 模式 *，且* 叢集最初是在 ad 模式中設定，則不適用此問題。

您可以藉由執行 [HgsServer](https://technet.microsoft.com/library/mt652162.aspx)來確認 HGS 伺服器的證明模式。

## <a name="memory-dump-encryption-policies"></a>記憶體傾印加密原則

如果您嘗試設定記憶體傾印加密原則，但未看到預設的 HGS 傾印原則 (Hgs \_ NoDumps、hgs \_ DumpEncryption 和 hgs \_ DumpEncryptionKey) 或傾印原則 Cmdlet (HgsAttestationDumpPolicy) ，則可能是您未安裝最新的累積更新。
若要修正此問題，請將 [您的 HGS 伺服器更新](guarded-fabric-manage-hgs.md#patching-hgs) 為最新的累積 Windows update，並 [啟用新的證明原則](guarded-fabric-manage-hgs.md#updates-requiring-policy-activation)。
在啟動新的證明原則之前，請確定您將 Hyper-v 主機更新為相同的累積更新，因為未安裝新傾印加密功能的主機在啟用 HGS 原則之後可能會通過證明。

## <a name="endorsement-key-certificate-error-messages"></a>簽署金鑰憑證錯誤訊息

使用 [HgsAttestationTpmHost](/powershell/module/hgsattestation/add-hgsattestationtpmhost) Cmdlet 註冊主機時，會從提供的平臺識別碼檔案解壓縮兩個 TPM 識別碼：簽署金鑰憑證 (EKcert) 和公開簽署金鑰 (EKpub) 。
EKcert 會識別 TPM 的製造商，以保證 TPM 是真實的，並透過一般供應鏈製造。
EKpub 可唯一識別該特定 TPM，而這是 HGS 用來授與主機存取權來執行受防護 Vm 的其中一個量值。

如果下列兩個條件的任一條件成立，您將會在註冊 TPM 主機時收到錯誤：
1. 平臺識別碼檔案 **未** 包含簽署金鑰憑證
2. 平臺識別碼檔案包含簽署金鑰憑證，但該憑證不會在您的系統上受到**信任**

某些 TPM 製造商未在其 Tpm 中包含 EKcerts。
如果您懷疑 TPM 是這種情況，請向您的 OEM 確認 Tpm 不應該有 EKcert，並使用 `-Force` 旗標以手動向 HGS 註冊主機。
如果您的 TPM 應該有 EKcert，但在平臺識別碼檔案中找不到，則請確定您使用的是系統管理員 (在主機上執行 [PlatformIdentifier](/powershell/module/platformidentifier/get-platformidentifier) 時提高許可權) PowerShell 主控台。

如果您收到 EKcert 未受信任的錯誤，請確定您已在每部 HGS 伺服器上 [安裝受信任的 tpm 根憑證套件](guarded-fabric-install-trusted-tpm-root-certificates.md) ，且 TPM 廠商的根憑證存在於本機電腦的 **TrustedTPM \_ >rootca.pem** 存放區中。 任何適用的中繼憑證也都必須安裝在本機電腦上的 **TrustedTPM \_ IntermediateCA** 存放區中。
安裝根和中繼憑證之後，您應該能夠 `Add-HgsAttestationTpmHost` 順利執行。

## <a name="group-managed-service-account-gmsa-privileges"></a>群組受管理的服務帳戶 (gMSA) 許可權

在 IIS 中用於金鑰保護服務應用程式集區的 HGS 服務帳戶 (gMSA) 必須被授與 [產生安全性審核](/windows/security/threat-protection/security-policy-settings/generate-security-audits) 許可權，也稱為 `SeAuditPrivilege` 。 如果缺少此許可權，則初始 HGS 設定會成功，且 IIS 會啟動，但金鑰保護服務無法運作，且會傳回 HTTP 錯誤 500 _ ( 「/KeyProtection 應用程式中的伺服器錯誤」 ) 。_ 您也可能會在應用程式事件記錄檔中看到下列警告訊息。
```
System.ComponentModel.Win32Exception (0x80004005): A required privilege is not held by the client
at Microsoft.Windows.KpsServer.Common.Diagnostics.Auditing.NativeUtility.RegisterAuditSource(String pszSourceName, SafeAuditProviderHandle& phAuditProvider)
at Microsoft.Windows.KpsServer.Common.Diagnostics.Auditing.SecurityLog.RegisterAuditSource(String sourceName)
```
或
```
Failed to register the security event source.
   at System.Web.HttpApplicationFactory.EnsureAppStartCalledForIntegratedMode(HttpContext context, HttpApplication app)
   at System.Web.HttpApplication.RegisterEventSubscriptionsWithIIS(IntPtr appContext, HttpContext context, MethodInfo[] handlers)
   at System.Web.HttpApplication.InitSpecial(HttpApplicationState state, MethodInfo[] handlers, IntPtr appContext, HttpContext context)
   at System.Web.HttpApplicationFactory.GetSpecialApplicationInstance(IntPtr appContext, HttpContext context)
   at System.Web.Hosting.PipelineRuntime.InitializeApplication(IntPtr appContext)

Failed to register the security event source.
   at Microsoft.Windows.KpsServer.Common.Diagnostics.Auditing.SecurityLog.RegisterAuditSource(String sourceName)
   at Microsoft.Windows.KpsServer.Common.Diagnostics.Auditing.SecurityLog.ReportAudit(EventLogEntryType eventType, UInt32 eventId, Object[] os)
   at Microsoft.Windows.KpsServer.KpsServerHttpApplication.Application_Start()

A required privilege is not held by the client
   at Microsoft.Windows.KpsServer.Common.Diagnostics.Auditing.NativeUtility.RegisterAuditSource(String pszSourceName, SafeAuditProviderHandle& phAuditProvider)
   at Microsoft.Windows.KpsServer.Common.Diagnostics.Auditing.SecurityLog.RegisterAuditSource(String sourceName)
```
此外，您可能會注意到沒有任何金鑰保護服務 Cmdlet (例如 [HgsKeyProtectionCertificate](/powershell/module/hgskeyprotection/get-hgskeyprotectioncertificate)) 工作，而是傳回錯誤。

若要解決此問題，您必須授與 gMSA 「產生安全性審核」 (SeAuditPrivilege) 。 若要這樣做，您可以 `SecPol.msc` 在 HGS 叢集的每個節點上使用本機安全性原則，或群組原則。 或者，您可以使用 [SecEdit.exe](/windows-server/administration/windows-commands/secedit) 工具來匯出目前的安全性原則、在設定檔中進行必要的編輯 (這是純文字) 然後再將它匯入。

> [!NOTE]
> 設定此設定時，針對許可權定義的安全性原則清單會完整覆寫預設值 (不會串連) 。 因此，在定義此原則設定時，請務必同時包含此許可權的預設持有者 (網路服務和本機服務) ，以及您要新增的 gMSA。

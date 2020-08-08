---
title: 針對主機守護者服務進行疑難排解
ms.topic: article
ms.assetid: 424b8090-0692-49a6-9dc4-3c0e77d74b80
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 09/25/2019
ms.openlocfilehash: 21c29c8432d9f578a50130719c61a255fdb5c649
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944078"
---
# <a name="troubleshooting-the-host-guardian-service"></a>針對主機守護者服務進行疑難排解

> 適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

本主題說明在受防護網狀架構中部署或操作主機守護者服務 (HGS) 伺服器時所遇到常見問題的解決方式。
如果您不確定問題的本質，請先嘗試在您的 HGS 伺服器和 Hyper-v 主機上執行受防護的網狀[架構診斷](guarded-fabric-troubleshoot-diagnostics.md)，以縮小可能的原因。

## <a name="certificates"></a>憑證

HGS 需要數個憑證才能操作，包括系統管理員設定的加密和簽署憑證，以及由 HGS 本身管理的證明憑證。
如果這些憑證設定不正確，HGS 將無法提供來自 Hyper-v 主機的要求，以證明或解除鎖定受防護 Vm 的金鑰保護裝置。
下列各節涵蓋與 HGS 上設定的憑證相關的常見問題。

### <a name="certificate-permissions"></a>憑證許可權

HGS 必須能夠存取憑證指紋新增至 HGS 的加密和簽署憑證的公開和私密金鑰。
具體而言，執行 HGS 服務 (gMSA) 群組受管理的服務帳戶需要存取金鑰。
若要尋找 HGS 使用的 gMSA，請在您的 HGS 伺服器上提高許可權的 PowerShell 提示字元中執行下列命令：

```powershell
(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName
```

授與 gMSA 帳戶存取權以使用私密金鑰的方式，取決於金鑰的儲存位置：電腦上的本機憑證檔案、硬體安全模組 (HSM) 或使用自訂的協力廠商金鑰儲存提供者。

#### <a name="grant-access-to-software-backed-private-keys"></a>授與軟體支援的私密金鑰存取權

如果您使用的是自我簽署憑證或憑證授權單位單位所發行的憑證**未**儲存在硬體安全性模組或自訂金鑰儲存提供者中，您可以執行下列步驟來變更私密金鑰許可權：

1. 開啟本機憑證管理員 (certlm.msc) 
2. 展開 [**個人 > 憑證**]，並尋找您要更新的簽署或加密憑證。
3. 以滑鼠右鍵按一下憑證，然後選取 [所有工作] **> 管理私密金鑰**]。
4. 按一下 **[新增]** ，將憑證的私密金鑰存取權授與新的使用者。
5. 在 [物件選擇器] 中，輸入先前找到的 HGS 的 gMSA 帳戶名稱，然後按一下 **[確定]**。
6. 請確定 gMSA 具有憑證的**讀取**許可權。
7. 按一下 **[確定**] 以關閉 [許可權] 視窗。

如果您是在 Server Core 上執行 HGS，或從遠端管理伺服器，您將無法使用本機憑證管理員來管理私密金鑰。
相反地，您必須下載可讓您在 PowerShell 中管理許可權的受防護網狀[架構工具 PowerShell 模組](https://www.powershellgallery.com/packages/GuardedFabricTools)。

1. 在 Server Core 機器上開啟已提升許可權的 PowerShell 主控台，或使用具有 HGS 本機系統管理員許可權的帳戶來使用 PowerShell 遠端處理。
2. 執行下列命令來安裝受防護的網狀架構工具 PowerShell 模組，並將私密金鑰的存取權授與 gMSA 帳戶。

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

如果您的憑證私密金鑰受到硬體安全模組的支援 (HSM) 或自訂金鑰儲存提供者 (KSP) ，則許可權模型將取決於您的特定軟體廠商。
如需最佳結果，請參閱廠商的檔或支援網站，以取得如何針對特定裝置/軟體處理私密金鑰許可權的相關資訊。
在所有情況下，HGS 使用的 gMSA 都需要加密、簽署和通訊憑證私密金鑰的*讀取*許可權，才能執行簽署和加密作業。

某些硬體安全模組不支援將私密金鑰的存取權授與特定的使用者帳戶;相反地，它們允許電腦帳戶存取特定金鑰集中的所有金鑰。
對於這類裝置，通常會有足夠的許可權可讓電腦存取您的金鑰，而 HGS 則可以利用該連線。

**Hsm 的秘訣**

以下是建議的設定選項，可協助您根據 Microsoft 及其合作夥伴的經驗，成功地將 HSM 支援的金鑰與 HGS 搭配使用。
這些秘訣是為了方便您而提供，並不保證在閱讀時正確，也不會被 HSM 製造商背書。
如果您有進一步的問題，請洽詢您的 HSM 製造商，以取得與特定裝置相關的精確資訊。

HSM 品牌/系列      | 建議
----------------------|-------------
Gemalto SafeNet       | 請確定憑證要求檔案中的 [金鑰使用方法] 屬性已設為 [0xa0]，以允許憑證用於簽署和加密。 此外，您必須使用本機憑證管理員工具將私密金鑰的*讀取*權授與 gMSA 帳戶， (參閱上述) 的步驟。
nCipher nShield        | 請確定每個 HGS 節點都可以存取包含簽署和加密金鑰的安全性世界。 您可能還需要使用本機憑證管理員，將 gMSA 的*讀取*許可權授與私密金鑰， (請參閱上述) 的步驟。
Utimaco CryptoServers | 請確定憑證要求檔案中的 [金鑰使用方法] 屬性已設定為 [0x13]，以允許憑證用於加密、解密和簽署。

### <a name="certificate-requests"></a>憑證要求

如果您要使用憑證授權單位單位，在 (PKI) 環境的公開金鑰基礎結構中發行憑證，您必須確定您的憑證要求包含 HGS 使用這些金鑰的最低需求。

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

如果您使用 Active Directory 憑證服務] (ADCS) 憑證範本來建立憑證，建議您使用具有下列設定的範本：

ADCS 範本屬性 | 必要值
-----------------------|---------------
提供者分類      | 金鑰儲存提供者
演算法名稱         | RSA
最小金鑰大小       | 2048
目的                | 簽章和加密
金鑰使用方式延伸    | 數位簽章、金鑰解碼、資料加密 ( 「允許加密使用者資料」 ) 


### <a name="time-drift"></a>時間漂移

如果您的伺服器時間與其他 HGS 節點或您的受防護網狀架構中的 Hyper-v 主機明顯漂移，您可能會遇到證明簽署者憑證有效性的問題。
證明簽署者憑證會在 HGS 的幕後建立和更新，並用於簽署證明服務發行給受防護主機的健康情況憑證。

若要重新整理證明簽署者憑證，請在提升許可權的 PowerShell 提示字元中執行下列命令。

```powershell
Start-ScheduledTask -TaskPath \Microsoft\Windows\HGSServer -TaskName
AttestationSignerCertRenewalTask
```

或者，您也可以手動執行排程工作，方法是開啟**工作排程器** (taskschd.msc]) ，流覽至**工作排程器程式庫 > Microsoft > Windows > HGSServer** ，並執行名為**AttestationSignerCertRenewalTask**的工作。

## <a name="switching-attestation-modes"></a>切換證明模式

如果您使用[HgsServer 指令程式](https://technet.microsoft.com/library/mt652180.aspx)將 HGS 從 TPM 模式切換到 Active Directory 模式，或反之亦然，可能需要10分鐘的時間，您的 HGS 叢集中的每個節點才會開始強制執行新的證明模式。
這是正常行為。
建議您不要從先前的證明模式移除任何允許主機的原則，除非您已確認所有主機都已使用新的證明模式證明成功。

**從 TPM 切換至 AD 模式時的已知問題**

如果您以 TPM 模式初始化 HGS 叢集，並在稍後切換到 Active Directory 模式，則會有一個已知的問題，讓 HGS 叢集中的其他節點無法切換到新的證明模式。
為確保所有 HGS 伺服器都強制執行正確的證明模式，請 `Set-HgsServer -TrustActiveDirectory` 在 hgs 叢集的**每個節點上**執行。
如果您是從 TPM 模式切換到 AD 模式 *，而*叢集原本是以 ad 模式設定，則不適用此問題。

您可以藉由執行[HgsServer](https://technet.microsoft.com/library/mt652162.aspx)來確認 HGS 伺服器的證明模式。

## <a name="memory-dump-encryption-policies"></a>記憶體傾印加密原則

如果您嘗試設定記憶體傾印加密原則，但看不到預設的 HGS 傾印原則 (Hgs \_ NoDumps、hgs \_ DumpEncryption 和 hgs \_ DumpEncryptionKey) 或傾印原則 Cmdlet (Add-HgsAttestationDumpPolicy) ，您可能未安裝最新的累積更新。
若要修正此問題，請將[您的 HGS 伺服器更新](guarded-fabric-manage-hgs.md#patching-hgs)為最新的累積 Windows 更新，並[啟動新的證明原則](guarded-fabric-manage-hgs.md#updates-requiring-policy-activation)。
啟動新的證明原則之前，請確定您已將 Hyper-v 主機更新為相同的累積更新，因為未安裝新傾印加密功能的主機在啟用 HGS 原則之後，可能會失敗證明。

## <a name="endorsement-key-certificate-error-messages"></a>簽署金鑰憑證錯誤訊息

使用[HgsAttestationTpmHost](https://docs.microsoft.com/powershell/module/hgsattestation/add-hgsattestationtpmhost) Cmdlet 來註冊主機時，會從提供的平臺識別碼檔案中解壓縮兩個 TPM 識別碼：簽署金鑰憑證 (EKcert) ，而 (EKpub) 的公開簽署金鑰。
EKcert 會識別 TPM 的製造商，藉由正常供應鏈來確保 TPM 的真實性和製造。
EKpub 會唯一識別該特定 TPM，而是 HGS 用來授與主機存取權以執行受防護 Vm 的其中一個量值。

當下列兩個條件之一成立時，您會在註冊 TPM 主機時收到錯誤：
1. 平臺識別碼檔案不**包含簽署**金鑰憑證
2. 平臺識別碼檔案包含簽署金鑰憑證，但該憑證在您的系統上**不受信任**

某些 TPM 製造商不會在其 Tpm 中包含 EKcerts。
如果您懷疑 TPM 發生這種情況，請向您的 OEM 確認您的 Tpm 不應該有 EKcert，並使用旗標 `-Force` 手動向 HGS 註冊主機。
如果您的 TPM 應該有 EKcert，但在平臺識別碼檔案中找不到，則請確定您使用的是系統管理員，在主機上執行[PlatformIdentifier](https://docs.microsoft.com/powershell/module/platformidentifier/get-platformidentifier)時， (提高許可權的) PowerShell 主控台。

如果您收到 EKcert 不受信任的錯誤，請確定您已在每部 HGS 伺服器上[安裝受信任的 tpm 根憑證封裝](guarded-fabric-install-trusted-tpm-root-certificates.md)，而且 TPM 廠商的根憑證存在於本機電腦的**TrustedTPM \_ rootca.cer**存放區中。 任何適用的中繼憑證也必須安裝在本機電腦上的**TrustedTPM \_ IntermediateCA**存放區中。
安裝根和中繼憑證之後，您應該能夠 `Add-HgsAttestationTpmHost` 順利執行。

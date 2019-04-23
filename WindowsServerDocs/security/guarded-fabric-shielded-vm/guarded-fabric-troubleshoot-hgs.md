---
title: 疑難排解 「 主機守護者服務
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 424b8090-0692-49a6-9dc4-3c0e77d74b80
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: 2dc9a612fa9760a6ca5f05efe1c287fd0872a1d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861249"
---
# <a name="troubleshooting-the-host-guardian-service"></a>疑難排解 「 主機守護者服務

> 適用於：Windows Server （半年通道），Windows Server 2016

本主題說明部署，或在受防護網狀架構中的主機守護者服務 (HGS) 伺服器的作業時所遇到的常見問題的解決方法。
如果您不確定您的問題，第一次嘗試執行的性質[受防護網狀架構診斷](guarded-fabric-troubleshoot-diagnostics.md)上您的 HGS 伺服器和 HYPER-V 主機，以縮小可能會導致。

## <a name="certificates"></a>憑證

HGS 需要數個憑證才能運作，包括系統管理員已設定的加密和簽署憑證，以及證明憑證管理本身的 HGS。
如果這些憑證的設定不正確，HGS 將無法在想要證明客戶資料或解除鎖定受防護 Vm 的金鑰保護裝置的 HYPER-V 主機的要求提供服務。
下列各節涵蓋常見與 HGS 上設定的憑證相關的問題。

### <a name="certificate-permissions"></a>憑證權限

HGS 必須能夠存取這兩個公用和私用金鑰的加密和簽署憑證新增至 HGS 的憑證指紋。
具體而言，群組受管理執行的 HGS 服務的服務帳戶 (gMSA) 需要存取金鑰。
HGS 伺服器上尋找 HGS 使用 gMSA，請在提升權限的 PowerShell 提示字元中執行下列命令：

```powershell
(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName
```

您授與 gMSA 帳戶使用的存取權的私用金鑰的方式取決於儲存金鑰的位置： 在本機憑證檔，在硬體安全性模組 (HSM)，或使用自訂的協力廠商金鑰儲存提供者的電腦上。

#### <a name="grant-access-to-software-backed-private-keys"></a>以軟體為基礎的私用金鑰授與存取權

如果您使用自我簽署的憑證或是由憑證授權單位所核發的憑證**不**儲存在硬體安全性模組或自訂金鑰儲存提供者，您可以變更私用金鑰的權限執行下列步驟：

1. 開啟本機憑證管理員 (certlm.msc)
2. 依序展開**個人 > 憑證**並尋找您想要更新的簽章或加密憑證。
3. 以滑鼠右鍵按一下憑證，然後選取**所有的工作 > 管理私用金鑰**。
4. 按一下 **新增**授與新的使用者存取憑證的私密金鑰。
5. 在物件選擇器中，輸入 HGS 稍早找到的 gMSA 帳戶名稱，然後按一下 **確定**。
6. 請確定具有 gMSA**讀取**憑證的存取權。
7. 按一下 **確定**以關閉 權限 視窗。

如果您在 Server Core 上執行 HGS，或從遠端管理伺服器，您將無法管理使用本機憑證管理員的私密金鑰。
相反地，您必須下載[受防護網狀架構工具 PowerShell 模組](https://www.powershellgallery.com/packages/GuardedFabricTools)這樣能讓您管理在 PowerShell 中的權限。

1. 開啟提升權限的 PowerShell 主控台在的 Server Core 電腦上或使用 PowerShell 遠端執行功能對 HGS 的本機系統管理員權限的帳戶。
2. 執行下列命令來安裝的受防護網狀架構工具 PowerShell 模組，並授與 gMSA 帳戶的私密金鑰。

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

#### <a name="grant-access-to-hsm-or-custom-provider-backed-private-keys"></a>授與 HSM 或自訂提供者支援的私用金鑰的存取權

如果您的憑證的私密金鑰受到硬體安全性模組 (HSM) 或自訂金鑰儲存提供者 (KSP)，權限模型取決於您的特定軟體廠商。
為了獲得最佳結果，請參閱廠商的文件或支援站台的特定裝置/軟體如何私用金鑰處理權限的詳細資訊。

有些硬體安全性模組不支援授與特定使用者帳戶的存取權的私用金鑰;相反地，它們會在一組特定的索引鍵中允許電腦帳戶對所有金鑰的存取。
對於這類裝置，通常足以讓您的金鑰的電腦存取權，因此 HGS 能夠運用該連線。

**Hsm 的秘訣**

以下是建議的組態選項，以協助您順利使用 hsm 型金鑰與 HGS Microsoft 和其合作夥伴的經驗。
祕訣可供您方便參考，並不保證能夠正確當時的讀取，或它們所背書的 HSM 製造商。
如有關您特定的裝置，如果您有進一步的問題的精確資訊，請連絡您的 HSM 製造商。

HSM 品牌/系列      | 建議
----------------------|-------------
Gemalto SafeNet       | 請確定金鑰使用方式屬性，在憑證要求檔案設定為 0xa0，允許用於簽署和加密憑證。 此外，您必須授與 gMSA 帳戶*讀取*使用本機憑證管理員工具的私密金鑰存取權 （請參閱上述步驟）。
Thales nShield        | 請確定每一個 HGS 節點可存取包含簽署和加密金鑰的安全園地。 您不需要設定 gMSA 特定權限。
Utimaco CryptoServers | 確定金鑰使用方式屬性，在憑證要求檔案設定為 0x13 等，讓要用於加密、 解密和簽署的憑證。

### <a name="certificate-requests"></a>憑證要求

如果您使用的憑證授權單位發出憑證的公開金鑰基礎結構 (PKI) 環境中，您必須確認您的憑證要求中包含這些機碼的 HGS 的使用方式的最低需求。

**簽署憑證**

CSR 屬性 | 必要的值
-------------|---------------
演算法    | RSA
金鑰大小     | 至少 2048 位元
金鑰使用方法    | Signature/Sign/DigitalSignature

**加密憑證**

CSR 屬性 | 必要的值
-------------|---------------
演算法    | RSA
金鑰大小     | 至少 2048 位元
金鑰使用方法    | Encryption/Encrypt/DataEncipherment

**Active Directory 憑證服務範本**

如果您使用 Active Directory 憑證服務 (ADCS) 憑證範本來建立建議您的憑證會使用範本，使用下列設定：

ADCS Template 屬性 | 必要的值
-----------------------|---------------
提供者類別目錄      | 金鑰儲存提供者
演算法名稱         | RSA
最小金鑰大小       | 2048
用途                | 簽章和加密
金鑰使用方式延伸模組    | 數位簽章，金鑰加密資料編密 （「 允許加密使用者資料 」）


### <a name="time-drift"></a>時間漂移

如果您的伺服器時間已從其他 HGS 節點或您受防護網狀架構中的 HYPER-V 主機的大幅漂移，您可能會遇到與證明簽署者憑證的有效性問題。
建立和更新的幕後 HGS 證明簽章者憑證和用來簽署發給證明服務的受防護主機的健康情況憑證。

若要重新整理證明簽章者憑證，請在提升權限的 PowerShell 提示字元中執行下列命令。

```powershell
Start-ScheduledTask -TaskPath \Microsoft\Windows\HGSServer -TaskName 
AttestationSignerCertRenewalTask
```

或者，您可以手動執行排定的工作 installationdirectory**工作排程器**(taskschd.msc)，瀏覽至**工作排程器程式庫 > Microsoft > Windows > HGSServer**和執行名為 「 任務**AttestationSignerCertRenewalTask**。

## <a name="switching-attestation-modes"></a>切換證明模式

如果從 TPM 模式切換至 Active Directory 模式，或反之亦然使用 HGS[組 HgsServer](https://technet.microsoft.com/library/mt652180.aspx) cmdlet，它可能需要 10 分鐘，每個節點 HGS 叢集中，以啟動 強制執行新的證明模式中。
這是正常的行為。
建議您不要移除任何原則，允許從先前的證明模式的主機，直到您已確認所有主機都證明成功使用新的證明模式。

**從 TPM 切換到廣告模式時的已知的問題**

如果您初始化到 Active Directory 模式中 TPM 模式和更新版本的交換器的 HGS 叢集，則會防止您的 HGS 叢集中其他節點切換到新的證明模式的已知的問題。
若要確保所有的 HGS 伺服器強制執行正確的證明模式，請執行`Set-HgsServer -TrustActiveDirectory`**每個節點上**的 HGS 叢集。
如果您從 TPM 模式切換至 AD 模式則不適用此問題*和*叢集原本設定在 AD 模式。

您可以執行，以確認您的 HGS 伺服器的證明模式[Get HgsServer](https://technet.microsoft.com/library/mt652162.aspx)。

## <a name="memory-dump-encryption-policies"></a>記憶體傾印加密原則

如果您嘗試要設定的記憶體傾印加密原則並不會看到預設 HGS 傾印原則 (Hgs\_NoDumps、 Hgs\_DumpEncryption 和 Hgs\_DumpEncryptionKey) 或傾印原則 cmdlet （Add-HgsAttestationDumpPolicy)，很可能您沒有安裝最新累積更新。
若要解決此問題，[更新您的 HGS 伺服器](guarded-fabric-manage-hgs.md#patching-hgs)最新累計 Windows update 並[啟用新的證明原則](guarded-fabric-manage-hgs.md#updates-requiring-policy-activation)。
請確定您更新相同的累計更新，然後再啟動新的證明原則，您的 HYPER-V 主機，因為一旦啟動 HGS 原則後，不需要安裝新傾印加密功能的主機可能會失敗證明。

## <a name="endorsement-key-certificate-error-messages"></a>簽署金鑰憑證錯誤訊息

註冊使用的主機時[新增 HgsAttestationTpmHost](https://docs.microsoft.com/powershell/module/hgsattestation/add-hgsattestationtpmhost) cmdlet，從提供的平台識別項檔擷取 TPM 的兩個識別碼： 簽署金鑰憑證 (EKcert) 和公開簽署金鑰 (EKpub)。
EKcert 識別製造商的 TPM，提供 TPM 是真確且製造透過一般的供應鏈的保證。
EKpub 可以唯一識別該特定 TPM，而且是其中一個 HGS 用來授與主應用程式存取權以執行受防護的 Vm 的量值。

註冊 TPM 主機，如果其中一個的兩個條件成立時，您會收到錯誤：
1. 平台識別項檔**則否**包含簽署金鑰憑證
2. 平台識別項檔包含簽署金鑰的憑證，但該憑證是否**不受信任**您系統上

特定的 TPM 製造商不包含 EKcerts 其 Tpm 中。
如果您懷疑這是您的 TPM 的情況，請確認鴗您鎏 OEM 應該不具有 EKcert 並使用您的 Tpm`-Force`旗標，以手動方式與 HGS 註冊主機。
如果您的 TPM 應有 EKcert 但平台識別項檔中找不到其中一個，請確定您使用的系統管理員 （提升權限） 的 PowerShell 主控台，但執行[Get PlatformIdentifier](https://docs.microsoft.com/powershell/module/platformidentifier/get-platformidentifier)主機上。

如果您收到您 EKcert 不受信任的錯誤時，請確定您有[安裝受信任的 TPM 根憑證封裝](guarded-fabric-install-trusted-tpm-root-certificates.md)上每一部 HGS 伺服器，您的 TPM 廠商的根憑證位於本機電腦的**TrustedTPM\_RootCA**儲存。 所有適用的中繼憑證也必須安裝在**TrustedTPM\_IntermediateCA**儲存在本機電腦上。
安裝根和中繼憑證之後，您應該能夠執行`Add-HgsAttestationTpmHost`成功。

---
title: 裝置健康情況證明
ms.topic: article
ms.assetid: 8e7b77a4-1c6a-4c21-8844-0df89b63f68d
author: brianlic-msft
ms.author: brianlic
ms.date: 10/12/2016
ms.openlocfilehash: 8ed6e2aafeeca0486bdb45019ba879e391af9934
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87936730"
---
# <a name="device-health-attestation"></a>裝置健康情況證明

>適用於：Windows Server 2016

Windows 10 (版本 1507) 已引進「裝置健康情況證明」(DHA)，其中包括：

-    與 Windows 10「行動裝置管理」(MDM) 架構整合，且符合 [Open Mobile Alliance (OMA) 標準](http://openmobilealliance.org/)。

-    支援以韌體或特定格式佈建「信賴模組平台」(TPM) 的裝置。

-    讓企業能夠提升其組織的安全性基準提升至硬體監視和安全性證明，而不影響作業成本或只有些微影響。

從 Windows Server 2016 開始，您現在可以將 DHA 服務當作組織內的伺服器角色來執行。 使用本主題來了解如何安裝及設定「裝置健康情況證明」伺服器角色。

## <a name="overview"></a>概觀

您可以使用 DHA 評估下列裝置健康情況：

-    支援 TPM 1.2 或 2.0 的 Windows 10 和 Windows 10 行動裝置版裝置。
-    由使用具備網際網路存取的 Active Directory 管理的內部部署裝置、由使用不具備網際網路存取的 Active Directory 管理的裝置、由 Azure Active Directory 管理的裝置，或者是使用 Active Directory 和 Azure Active Directory 兩者的混合式部署。


### <a name="dha-service"></a>DHA 服務

DHA 服務會驗證裝置的 TPM 和 PCR 記錄，然後發行 DHA 報告。 Microsoft 以三種方式提供 DHA 服務︰

- **DHA 雲端服務**  Microsoft 管理的免費 DHA 服務，已進行地理區域負載平衡，並且已針對從世界各地進行存取最佳化。

- **DHA 內部部署服務** 在 Windows Server 2016 中引進的新伺服器腳色。 擁有 Windows Server 2016 授權的客戶可以免費取得。

- **DHA Azure 雲端服務** Microsoft Azure 中的虛擬主機。 若要這樣做，您需要 DHA 內部部署服務的虛擬主機和授權。

DHA 服務與 MDM 解決方案整合，並提供下列功能︰

-    將它們從裝置收到的資訊 (透過現有裝置管理通訊通道) 與 DHA 報告結合
-    根據硬體證明和受保護的資料，做出更安全且可靠的安全性決策

以下範例說明您可以如何使用 DHA 來協助提升您組織資產的安全性保護基準。

1. 您可以建立會檢查下列開機組態/屬性的原則：
   - 安全開機
   - BitLocker
   - ELAM
2. MDM 解決方案會強制執行此原則並根據 DHA 報告資料觸發矯正動作。  例如，它可以驗證下列項目：
   - 已啟用「安全開機」、裝置已載入經驗證且可信任的程式碼，且 Windows 開機載入器未遭竄改。
   - 「信任式開機」已成功驗證 Windows 核心的數位簽章及裝置啟動時載入的元件。
   - 「測量開機」已建立受 TPM 保護的稽核線索，並可從遠端驗證。
   - 已啟用 BitLocker，且它會在裝置關閉時保護資料。
   - ELAM 會在早期開機階段啟用並且會監視執行階段。

#### <a name="dha-cloud-service"></a>DHA 雲端服務

DHA 雲端服務提供下列優點：

-    檢閱從在 MDM 解決方案中註冊的裝置處收到的 TCG 和 PCR 裝置開機記錄。
-    根據裝置的 TPM 晶片所收集並保護的資料，建立描述裝置啟動方式，且可以防止竄改及可辨識其是否遭竄改的報告 (DHA 報告)。
-    在已受保護的通訊通道中將 DHA 報告傳遞給要求該報告的 MDM 伺服器。

#### <a name="dha-on-premises-service"></a>DHA 內部部署服務

DHA 內部部署服務提供所有 DHA 雲端服務提供的功能。  它也可讓客戶︰

 - 在您自有的資料中心執行 DHA 服務以獲得最佳效能
 - 確保 DHA 報告不會離開您的網路

#### <a name="dha-azure-cloud-service"></a>DHA Azure 雲端服務

此服務會提供等同 DHA 內部部署服務的功能，不過 DHA Azure 雲端服務是在 Microsoft Azure 中以虛擬主機的方式執行。

### <a name="dha-validation-modes"></a>DHA 驗證模式

您可以將 DHA 內部部署服務設定為以 EKCert 或 AIKCert 其中一種驗證模式執行。 當 DHA 服務發出報告時，它會指出報告是以 AIKCert 或 EKCert 驗證模式發出。 只要 EKCert 的信任鏈結維持在最新狀態，AIKCert 和 EKCert 驗證模式就能提供相同的安全性保證。

#### <a name="ekcert-validation-mode"></a>EKCert 驗證模式

EKCert 驗證模式已經針對未連線到網際網路的組織最佳化。 連線到以 EKCert 驗證模式執行之 DHA 服務的裝置「不會」**** 直接存取網際網路。

當 DHA 以 EKCert 驗證模式執行時，它是依賴企業管理的信任鏈結，必須偶爾更新 (每年大約 5-10 次)。

Microsoft 會在 .cab 封存中可公開存取的封存中，針對核准的 TPM 製造商發行 (在它們可用時) 受信任的根憑證和中繼 CA 的可彙總套件。 您需要下載該摘要，驗證其完整性，並將其安裝在執行「裝置健康情況證明」的伺服器上。

封存的範例為 [https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925) 。

#### <a name="aikcert-validation-mode"></a>AIKCert 驗證模式

AIKCert 驗證模式已經針對可存取網際網路的作業環境最佳化。 連線到 DHA 服務且執行 AIKCert 驗證模式的裝置必須可以直接存取網際網路，並且能從 Microsoft 取得 AIK 憑證。

## <a name="install-and-configure-the-dha-service-on-windows-server-2016"></a>在 Windows Server 2016 上安裝和設定 DHA 服務

使用以下章節在 Windows Server 2016 上安裝和設定 DHA。

### <a name="prerequisites"></a>必要條件

為了設定和驗證 DHA 內部部署服務，您需要：

- 執行 Windows Server 2016 的伺服器。
- 一 (或多) 部包含 TPM (1.2 或 2.0) 的 Windows 10 用戶端裝置，且已為執行最新的 Windows Insider 組件準備就緒。
- 決定您要以 EKCert 或 AIKCert 驗證模式執行。
- 下列憑證：
  - **DHA SSL 憑證** 鏈結到受信任的企業根憑證且包含可匯出私密金鑰的 x.509 SSL 憑證。 此憑證會保護 DHA 資料傳輸，包括伺服器對伺服器 (DHA 服務和 MDM 伺服器)，以及伺服器對用戶端 (DHA 服務和 Windows 10 裝置) 之間的通訊。
  - **DHA 簽署憑證** 鏈結到受信任的企業根憑證且包含可匯出私密金鑰的 x.509 憑證。 DHA 服務將此憑證用於數位簽署。
  - **DHA 加密憑證** 鏈結到受信任的企業根憑證且包含可匯出私密金鑰的 x.509 憑證。 DHA 服務也將此憑證用於加密。


### <a name="install-windows-server-2016"></a>安裝 Windows Server 2016

使用您想要的安裝方式 (例如「Windows 部署服務」) 安裝 Windows Server 2016，或從開機媒體、USB 磁碟機或本機檔案系統執行安裝程式。 如果這是您第一次設定 DHA 內部部署服務，您應該使用「桌面體驗」**** 安裝選項來安裝 Windows Server 2016。

### <a name="add-the-device-health-attestation-server-role"></a>新增裝置健康情況證明伺服器角色

您可以使用「伺服器管理員」來安裝「裝置健康情況證明」伺服器角色和其相依性。

安裝 Windows Server 2016 之後，裝置會重新啟動並開啟「伺服器管理員」。 如果「伺服器管理員」沒有自動啟動，請按一下 [開始]****，然後按一下 [伺服器管理員]****。

1.    按一下 [**新增角色及功能**]。
2.    在 [在您開始前]  頁面上，按一下 [下一步]  。
3.    在 [選取安裝類型]**** 頁面上，按一下 [角色型或功能型安裝]****，然後按 [下一步]****。
4.    在 [選取目的地伺服器]**** 頁面上，按一下 [從伺服器集區選取伺服器]****，選取伺服器然後按一下 [下一步]****。
5.    在 [選取伺服器角色]**** 頁面上，選取 [裝置健康情況證明]**** 核取方塊。
6.    按一下 [新增功能]**** 來安裝其他必要的角色服務和功能。
7.    按 [下一步]  。
8.    在 [選取功能]**** 頁面上，按 [下一步]****。
9.    在 [網頁伺服器 (IIS) 角色]**** 頁面上，按一下 [下一步]****。
10.    在 [選取角色服務]**** 頁面上，按 [下一步]****。
11.    在 [裝置健全狀況證明服務]**** 頁面上，按一下 [下一步]****。
12.    在 [確認安裝選項]**** 頁面上，按一下 [安裝]****。
13.    安裝完成時，按一下 [關閉]****。

### <a name="install-the-signing-and-encryption-certificates"></a>安裝簽署和加密憑證

使用下列 Windows PowerShell 指令碼安裝簽署和加密憑證。 如需指紋的詳細資訊，請參閱[如何：取出憑證的指紋](https://msdn.microsoft.com/library/ms734695.aspx)。

```
$key = Get-ChildItem Cert:\LocalMachine\My | Where-Object {$_.Thumbprint -like "<thumbprint>"}
$keyname = $key.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
$keypath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\" + $keyname
icacls $keypath /grant <username>`:R

#<thumbprint>: Certificate thumbprint for encryption certificate or signing certificate
#<username>: Username for web service app pool, by default IIS_IUSRS
```

### <a name="install-the-trusted-tpm-roots-certificate-package"></a>安裝受信任的 TPM 根憑證封裝

若要安裝受信任的 TPM 根憑證封裝，您必須將它解壓縮，移除任何您的組織不信任的信任鏈結，然後執行 setup.cmd。

#### <a name="download-the-trusted-tpm-roots-certificate-package"></a>下載受信任的 TPM 根憑證封裝

安裝憑證套件之前，您可以從下載最新的受信任 TPM 根目錄清單 [https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925) 。

> **重要事項：** 安裝封裝之前，請確認該封裝已由 Microsoft 以數位方式簽署。

#### <a name="extract-the-trusted-certificate-package"></a>解壓縮受信任的憑證封裝
執行下列命令來將受信任的憑證封裝解壓縮。
```
mkdir .\TrustedTpm
expand -F:* .\TrustedTpm.cab .\TrustedTpm
```

#### <a name="remove-the-trust-chains-for-tpm-vendors-that-are-not-trusted-by-your-organization-optional"></a>移除您的組織「不信任」** 的 TPM 廠商的信任鏈結 (選擇性)

刪除您的組織不信任的任何 TPM 廠商信任鏈結的資料夾。

> **注意：** 若使用 AIK 憑證模式，則需要 Microsoft 資料夾以驗證 Microsoft 發行的 AIK 憑證。

#### <a name="install-the-trusted-certificate-package"></a>安裝受信任的憑證封裝
執行 .cab 檔案中的設定指令碼來安裝受信任的憑證封裝。

```
.\setup.cmd
```

### <a name="configure-the-device-health-attestation-service"></a>設定裝置健康情況證明服務

您可以使用 Windows PowerShell 來設定 DHA 內部部署服務。

```
Install-DeviceHealthAttestation -EncryptionCertificateThumbprint <encryption> -SigningCertificateThumbprint <signing> -SslCertificateStoreName My -SslCertificateThumbprint <ssl> -SupportedAuthenticationSchema "<schema>"

#<encryption>: Thumbprint of the encryption certificate
#<signing>: Thumbprint of the signing certificate
#<ssl>: Thumbprint of the SSL certificate
#<schema>: Comma-delimited list of supported schemas including AikCertificate, EkCertificate, and AikPub
```

### <a name="configure-the-certificate-chain-policy"></a>設定憑證鏈結原則

執行以下 Windows PowerShell 指令碼來設定憑證鏈結原則。

```
$policy = Get-DHASCertificateChainPolicy
$policy.RevocationMode = "NoCheck"
Set-DHASCertificateChainPolicy -CertificateChainPolicy $policy
```

## <a name="dha-management-commands"></a>DHA 管理命令

以下是一些 Windows PowerShell 範例，可協助您管理 DHA 服務。

### <a name="configure-the-dha-service-for-the-first-time"></a>首次設定 DHA 服務

```
Install-DeviceHealthAttestation -SigningCertificateThumbprint "<HEX>" -EncryptionCertificateThumbprint "<HEX>" -SslCertificateThumbprint "<HEX>" -Force
```

### <a name="remove-the-dha-service-configuration"></a>移除 DHA 服務設定

```
Uninstall-DeviceHealthAttestation -RemoveSslBinding -Force
```
### <a name="get-the-active-signing-certificate"></a>取得作用中簽署憑證

```
Get-DHASActiveSigningCertificate
```
### <a name="set-the-active-signing-certificate"></a>設定作用中簽署憑證

```
Set-DHASActiveSigningCertificate -Thumbprint "<hex>" -Force
```

> **注意：** 此憑證必須部署在執行 DHA 服務的伺服器上的 **LocalMachine\My** 憑證存放區。 設定作用中簽署憑證之後，現有的作用中簽署憑證會移動到非作用中簽署憑證清單。

### <a name="list-the-inactive-signing-certificates"></a>列出非作用中簽署憑證
```
Get-DHASInactiveSigningCertificates
```

### <a name="remove-any-inactive-signing-certificates"></a>移除任何非作用中簽署憑證
```
Remove-DHASInactiveSigningCertificates -Force
Remove-DHASInactiveSigningCertificates  -Thumbprint "<hex>" -Force
```

> **注意：** 服務中一次只能存在「一個」** 非作用中憑證 (任何類型)。 當憑證已不再需要時，應將它們從非作用中憑證清單中移除。

### <a name="get-the-active-encryption-certificate"></a>取得作用中加密憑證

```
Get-DHASActiveEncryptionCertificate
```

### <a name="set-the-active-encryption-certificate"></a>設定作用中加密憑證

```
Set-DHASActiveEncryptionCertificate -Thumbprint "<hex>" -Force
```

此憑證必須部署在裝置中的 **LocalMachine\My** 憑證存放區。

設定作用中加密憑證之後，現有的作用中加密憑證會移動到非作用中加密憑證清單。

### <a name="list-the-inactive-encryption-certificates"></a>列出非作用中加密憑證

```
Get-DHASInactiveEncryptionCertificates
```
### <a name="remove-any-inactive-encryption-certificates"></a>移除任何非作用中加密憑證

```
Remove-DHASInactiveEncryptionCertificates -Force
Remove-DHASInactiveEncryptionCertificates -Thumbprint "<hex>" -Force
```

### <a name="get-the-x509chainpolicy-configuration"></a>取得 X509ChainPolicy 組態

```
Get-DHASCertificateChainPolicy
```
### <a name="change-the-x509chainpolicy-configuration"></a>變更 X509ChainPolicy 組態

```
$certificateChainPolicy = Get-DHASInactiveEncryptionCertificates
$certificateChainPolicy.RevocationFlag = <X509RevocationFlag>
$certificateChainPolicy.RevocationMode = <X509RevocationMode>
$certificateChainPolicy.VerificationFlags = <X509VerificationFlags>
$certificateChainPolicy.UrlRetrievalTimeout = <TimeSpan>
Set-DHASCertificateChainPolicy = $certificateChainPolicy
```

## <a name="dha-service-reporting"></a>DHA 服務報告

以下是 DHA 服務向 MDM 解決方案所回報訊息的清單：

- **200** HTTP 正常。 傳回憑證。
- **400**不正確的要求。 無效的要求格式、無效的健康情況憑證、憑證簽署不相符、無效的健康情況證明 Blob，或無效的健全狀態 Blob。 如回應結構描述所述，回應也包含訊息，其中包含可用於診斷的錯誤碼和錯誤訊息。
- **500**內部伺服器錯誤。 如果有導致服務無法發出憑證的問題，就可能會發生此情況。
- **503** 節流拒絕要求以防止伺服器超載。

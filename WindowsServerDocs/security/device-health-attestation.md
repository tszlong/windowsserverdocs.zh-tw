---
title: "裝置健康證明"
H1: na
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology:
- techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8e7b77a4-1c6a-4c21-8844-0df89b63f68d
author: brianlic-msft
ms.date: 10/12/2016
ms.openlocfilehash: d304ee3456f8db1e5b202c1d9221d1374a5251be
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="device-health-attestation"></a>裝置健康證明

>適用於：Windows Server 2016

在 Windows 10 版本 1507，裝置健康證明 (DHA) 包含下列動作：

-   整合的 Windows 10 行動裝置管理 (MDM) 架構，以配合[開放行動聯盟 (OMA) 標準](http://openmobilealliance.org/)。

-   支援的裝置已信賴模組平台 (TPM) 提供給在不同的格式或韌體。

-   可讓企業增加硬體組織的安全性監視和 attested 最少使用的安全性或不會影響作業成本。

開始使用 Windows Server 2016，您現在可以執行 DHA 服務伺服器角色與在組織中。 使用本主題以了解如何安裝及設定裝置健康證明伺服器角色。

## <a name="overview"></a>概觀

您可以使用 DHA 評估裝置健康適用於：
  
-   TPM 1.2 或 2.0 支援的 Windows 10 和「Windows 10 行動裝置版的裝置。  
-   先由使用 Active Directory 具有網際網路存取權，裝置使用 Active Directory 網際網路存取，而混合使用 Azure Active Directory 和 Active Directory 部署的 Azure Active Directory，受管理的裝置所管理的裝置。


### <a name="dha-service"></a>DHA 服務

DHA 服務驗證 TPM 和 PCR 登的裝置，然後問題 DHA 報告。 Microsoft 提供 DHA 服務三種方式：

- **DHA 雲端服務**A Microsoft 管理 DHA 服務，可免費、地理-負載平衡，以及適用於存取來自不同地區的世界最佳化。

- **DHA 先服務**在 Windows Server 2016 中引進了新的伺服器角色。 可用來針對 Windows Server 2016 授權已使用。

- **DHA Azure 雲端服務**Microsoft Azure virtual 主機。 若要這樣做，您需要 virtual 主機和授權 DHA 先服務。

DHA 服務整合 MDM 方案，並提供下列動作： 

-   結合時收到 DHA 報告（透過現有裝置管理的通訊通道）裝置的資訊
-   決定更安全且信賴的安全性，根據硬體 attested 和受保護的資料

以下是範例顯示如何使用 DHA 可協助您提高資產您組織的安全性防護列。

1. 您建立的原則，以檢查下列開機設定日屬性：
  - 安全開機
  - BitLocker
  - ELAM
2. 執行這項原則 MDM 方案，以及修正動作，根據 DHA 報告資料觸發程序。  例如，它可能驗證：
  - 安全開機的功能、裝置載入受信任的程式碼是正版，並不竄改 Windows 開機 loader。
  - 信任的成功驗證開機的 Windows 核心和元件載入時開始使用裝置的數位簽章。
  - 測量的開機建立 TPM 受稽核可能會遠端驗證。
  - BitLocker 已功能，並使其裝置已被時保護資料關閉。
  - ELAM 已功能的早期開機階段及監視執行階段。
  
#### <a name="dha-cloud-service"></a>DHA 雲端服務

DHA 雲端服務提供下列優點：

-   審查 TCG 和 PCR 裝置開機登收到從已退出 MDM 方案的裝置。 
-   建立竄改防並防明顯報告（DHA 報告），描述裝置如何開始使用根據受裝置的 TPM 晶片，所收集的資料。 
-   MDM 伺服器要求受保護的通訊通道報告傳送 DHA 報告。

#### <a name="dha-on-premises-service"></a>DHA 先服務

DHA 先服務提供提供 DHA 雲端服務的所有功能。  它也會讓針對到：

 - 將效能最佳化 DHA 服務執行資料中心
 - 請確定 DHA 報告不會讓您的網路

#### <a name="dha-azure-cloud-service"></a>DHA Azure 雲端服務

這項服務提供 DHA 先服務，相同的功能，除了 DHA Azure 雲端服務執行中 Microsoft Azure virtual 主機。

### <a name="dha-validation-modes"></a>DHA 驗證模式

您可以設定 DHA 先服務 EKCert 或 AIKCert 驗證模式執行。 當 DHA 服務問題報告時，表示它已發行 AIKCert 或 EKCert 驗證模式。 AIKCert 和 EKCert 驗證模式提供的相同的安全性保證，只要信任 EKCert 鏈處於最新狀態。

#### <a name="ekcert-validation-mode"></a>EKCert 驗證模式

EKCert 驗證模式最適合裝置在組織中未連接到網際網路。 裝置連接到執行 EKCert 驗證模式中的 DHA 服務執行**未**能直接存取網際網路。

DHA EKCert 驗證模式中執行時，它會依賴不定期的更新 (約 5-每 10 年倍) 必須信任管理企業鏈結。 

Microsoft 發行的受信任的根和中繼 CA 核准 TPM 製造商彙總的套件（推出時）在公開保存在.cab 保存。 您需要下載摘要、驗證其完整性，以及執行裝置健康證明的伺服器上安裝它。

範例保存是[https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab)。

#### <a name="aikcert-validation-mode"></a>AIKCert 驗證模式

AIKCert 驗證模式最適合作業環境具有網際網路存取權。 裝置連接到執行 AIKCert 驗證模式中的 DHA 服務必須直接存取網際網路，無法取得 Microsoft 的 AIK 憑證。 

## <a name="install-and-configure-the-dha-service-on-windows-server-2016"></a>安裝和 Windows Server 2016 上設定 DHA 服務

使用下列的各節取得 DHA 安裝和 Windows Server 2016 上的設定。

### <a name="prerequisites"></a>必要條件

設定並確認 DHA 先服務，您必須：

- 執行 Windows Server 2016 的伺服器。
- TPM（1.2 或 2.0）可在執行最新的 Windows 測試人員準備好清除日狀態的一（或多個）Windows 10 client 裝置組建。
- 如果您要用來執行 EKCert 或 AIKCert 驗證模式中的選擇。
- 以下的憑證：
  - **DHA SSL 憑證**到企業受信任的根鏈結匯出私密金鑰 x.509 SSL 憑證。 這個憑證保護 DHA 資料通訊傳輸包括伺服器（DHA 服務和 MDM 伺服器）和伺服器通訊 client（DHA 服務及 Windows 10 的裝置）。
  - **DHA 專屬的簽署憑證**x.509 匯出私密金鑰鏈結到企業受信任的根憑證。 DHA 服務會使用此憑證的數位簽章。 
  - **DHA 加密憑證**x.509 匯出私密金鑰鏈結到企業受信任的根憑證。 DHA 服務也會使用此憑證的加密。 


### <a name="install-windows-server-2016"></a>安裝 Windows Server 2016

安裝 Windows Server 2016 使用您的慣用的安裝方法，Windows 部署服務，例如或執行安裝程式可開機媒體、USB 磁碟機或在本機檔案系統。 如果這是您所設定的 DHA 先服務第一次，應該安裝 Windows Server 2016 使用**桌面體驗**安裝選項。

### <a name="add-the-device-health-attestation-server-role"></a>新增裝置健康證明伺服器角色

您可以藉由伺服器管理員安裝裝置健康證明伺服器角色及其相依性。 

您已安裝 Windows Server 2016 之後，裝置重新開機，開啟伺服器管理員。 如果在伺服器管理員不會自動開始，請按一下**[開始]**，然後按一下 [**伺服器管理員**。

1.  按一下**新增角色與功能**。
2.  在**在您開始之前**頁面上，按一下 [**下**。
3.  上**選取 [安裝類型**頁面上，按一下 [**以角色為基礎，或為基礎的功能的安裝**，然後按一下 [**下一步**。
4.  在**選擇目的伺服器**頁面上，按一下 [**選取伺服器伺服器集區的**，選取 [伺服器]，然後按一下**下一步**。
5.  在**選擇伺服器角色**頁面上，選取 [**裝置健康證明**核取方塊。
6.  按一下**[新增功能**來安裝其他所需的角色服務及功能。
7.  按一下**下一步**。
8.  在**選擇功能**頁面上，按一下 [**下**。
9.  在**網頁伺服器角色 (IIS)**頁面上，按**下**。
10. 在**選擇角色服務**頁面上，按一下 [**下**。
11. 在**裝置健康證明服務**頁面上，按一下 [**下**。
12. 在**確認安裝選項**頁面上，按**安裝**。
13. 安裝完成時，按**關閉**。

### <a name="install-the-signing-and-encryption-certificates"></a>簽署及加密憑證安裝

使用下列 Windows PowerShell 指令碼以安裝簽署及加密憑證。 如需指紋的詳細資訊，請查看[的方式：擷取的憑證指紋](https://msdn.microsoft.com/library/ms734695.aspx)。

```
$key = Get-ChildItem Cert:\LocalMachine\My | Where-Object {$_.Thumbprint -like "<thumbprint>"}
$keyname = $key.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
$keypath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\" + $keyname
icacls $keypath /grant <username>`:R
  
#<thumbprint>: Certificate thumbprint for encryption certificate or signing certificate
#<username>: Username for web service app pool, by default IIS_IUSRS
```

### <a name="install-the-trusted-tpm-roots-certificate-package"></a>安裝受信任的 TPM 根憑證套件

若要安裝的受信任的 TPM 根憑證套件，您必須將它、移除所有信任的鏈結由您的組織，不受信任，然後執行 [setup.cmd。

#### <a name="download-the-trusted-tpm-roots-certificate-package"></a>下載受信任的 TPM 根憑證套件

安裝套件的憑證之前，您可以下載最新的來自信任的 TPM 根清單[https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab)。

> **重要事項：**安裝套件，請先確認它以數位簽署 Microsoft。

#### <a name="extract-the-trusted-certificate-package"></a>擷取的受信任的憑證套件
執行下列命令解壓縮受信任的憑證套件。
```
mkdir .\TrustedTpm
expand -F:* .\TrustedTpm.cab .\TrustedTpm
```

#### <a name="remove-the-trust-chains-for-tpm-vendors-that-are-not-trusted-by-your-organization-optional"></a>移除信任鏈結 TPM 廠商的*未*受組織（選擇性）

Delete 任何不由您的組織受信任的 TPM 廠商信任鏈結的資料夾。

> **注意：** Microsoft 資料夾使用 AIK 憑證模式下，如果需要驗證 Microsoft 發出 AIK 憑證。

#### <a name="install-the-trusted-certificate-package"></a>安裝受信任的憑證套件
從.cab 執行安裝指令碼安裝受信任的憑證套件。

```
.\setup.cmd
```

### <a name="configure-the-device-health-attestation-service"></a>設定裝置健康證明服務

您可以使用 Windows PowerShell 來設定 DHA 先服務。

```
Install-DeviceHealthAttestation -EncryptionCertificateThumbprint <encryption> -SigningCertificateThumbprint <signing> -SslCertificateStoreName My -SslCertificateThumbprint <ssl> -SupportedAuthenticationSchema "<schema>"

#<encryption>: Thumbprint of the encryption certificate
#<signing>: Thumbprint of the signing certificate
#<ssl>: Thumbprint of the SSL certificate
#<schema>: Comma-delimited list of supported schemas including AikCertificate, EkCertificate, and AikPub
```

### <a name="configure-the-certificate-chain-policy"></a>設定的憑證鏈結原則

將憑證鏈結原則設定，執行下列 Windows PowerShell 指令碼。

```
$policy = Get-DHASCertificateChainPolicy
$policy.RevocationMode = "NoCheck"
Set-DHASCertificateChainPolicy -CertificateChainPolicy $policy
```

## <a name="dha-management-commands"></a>DHA 管理命令

以下是 Windows PowerShell 範例可協助您管理 DHA 服務。

### <a name="configure-the-dha-service-for-the-first-time"></a>設定 DHA 服務第一次

```
Install-DeviceHealthAttestation -SigningCertificateThumbprint "<HEX>" -EncryptionCertificateThumbprint "<HEX>" -SslCertificateThumbprint "<HEX>" -Force
```

### <a name="remove-the-dha-service-configuration"></a>移除 DHA 服務設定

```
Uninstall-DeviceHealthAttestation -RemoveSslBinding -Force
```
### <a name="get-the-active-signing-certificate"></a>取得使用專屬的簽署憑證

```
Get-DHASActiveSigningCertificate
```
### <a name="set-the-active-signing-certificate"></a>設定「active 專屬的簽署憑證

```
Set-DHASActiveSigningCertificate -Thumbprint "<hex>" -Force
```

> **注意：**這個憑證必須部署 DHA 服務執行的伺服器上**LocalMachine\My**憑證存放區。 使用中的專屬的簽署憑證設定時，使用現有專屬的簽署憑證移到非使用中的簽署憑證的清單。

### <a name="list-the-inactive-signing-certificates"></a>非使用中的簽署憑證的清單
```
Get-DHASInactiveSigningCertificates
```

### <a name="remove-any-inactive-signing-certificates"></a>移除所有非使用中的專屬的簽署憑證
```
Remove-DHASInactiveSigningCertificates -Force
Remove-DHASInactiveSigningCertificates  -Thumbprint "<hex>" -Force
```

> **注意：**只*一*（的任何類型）非使用中的憑證可能會在任何時候存在於服務。 憑證應該之後已不再需要的非使用中的憑證清單中移除。

### <a name="get-the-active-encryption-certificate"></a>取得使用中的加密憑證

```
Get-DHASActiveEncryptionCertificate
```

### <a name="set-the-active-encryption-certificate"></a>設定的使用中的加密憑證

```
Set-DHASActiveEncryptionCertificate -Thumbprint "<hex>" -Force
```

您必須在裝置上部署憑證**LocalMachine\My**憑證存放區。 

使用中的加密憑證設定時，使用現有的作用中的加密憑證移到非使用中的加密憑證的清單。

### <a name="list-the-inactive-encryption-certificates"></a>非使用中的加密憑證的清單

```
Get-DHASInactiveEncryptionCertificates
```
### <a name="remove-any-inactive-encryption-certificates"></a>移除所有非使用中的加密憑證

```
Remove-DHASInactiveEncryptionCertificates -Force
Remove-DHASInactiveEncryptionCertificates -Thumbprint "<hex>" -Force 
```

### <a name="get-the-x509chainpolicy-configuration"></a>取得 X509ChainPolicy 設定 

```
Get-DHASCertificateChainPolicy
```
### <a name="change-the-x509chainpolicy-configuration"></a>變更 X509ChainPolicy 設定

```
$certificateChainPolicy = Get-DHASInactiveEncryptionCertificates
$certificateChainPolicy.RevocationFlag = <X509RevocationFlag>
$certificateChainPolicy.RevocationMode = <X509RevocationMode>
$certificateChainPolicy.VerificationFlags = <X509VerificationFlags>
$certificateChainPolicy.UrlRetrievalTimeout = <TimeSpan>
Set-DHASCertificateChainPolicy = $certificateChainPolicy
```

## <a name="dha-service-reporting"></a>DHA 服務報告

以下是一份郵件回報 DHA 服務 MDM 方案： 

- **200** HTTP [確定]。 憑證會傳回。
- **400**錯誤的要求。 格式不正確的要求，健康無效的憑證，並不會憑證簽章相符項目、無效健康證明 Blob 或不正確的健康狀態 Blob。 回應也包含訊息，依回應區結構描述與錯誤碼的錯誤訊息，可用於診斷所述。
- **500**內部伺服器錯誤。 如果這情形避免服務發行憑證的問題。
- **503** Throttling 拒絕要求，以防止載伺服器。 

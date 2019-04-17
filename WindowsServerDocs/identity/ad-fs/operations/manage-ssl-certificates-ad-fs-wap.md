---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: "AD FS 和 Windows Server 2016 中的 WAP 管理 SSL 憑證"
description: "AD FS 和 Windows Server 2016 中的 WAP 管理 SSL 憑證"
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5156b3ad357ab8edb5e08a89a459beaf9b8c9b1a
ms.sourcegitcommit: ca7dc3d56a33924ae5fe0e9ffb1274da6dc4e54d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2017
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>AD FS 和 Windows Server 2016 中的 WAP 管理 SSL 憑證

>適用於：Windows Server 2016

此文章將描述 AD FS 和 WAP 的伺服器來部署新 SSL 憑證的方式。

>[!NOTE]
>接下來的 AD FS 發電廠 SSL 憑證的取代的建議的方式是使用 Azure AD 連接。  如需詳細資訊請查看[更新 Active Directory 同盟 Services (AD FS) 發電廠 SSL 憑證](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>取得您 SSL 憑證
對於 production AD FS 農場建議公開信任的 SSL 憑證。 這通常被取得提交憑證簽署要求 (CSR)，第三方、 公用憑證提供者。 有各種不同的方式產生 CSR，包括從 Windows 7 或更高的電腦。 您的供應商應該會有這樣的文件。

- 請確定憑證符合[AD FS 和 Web 應用程式 Proxy SSL 憑證需求](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>需要多少憑證
建議使用的常見 SSL 憑證所有 AD FS 和 Web 應用程式的 Proxy 伺服器上。 詳細的需求看到文件中的[AD FS 和 Web 應用程式 Proxy SSL 憑證需求](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="ssl-certificate-requirements"></a>SSL 憑證需求
包括命名需求根信任與擴充功能的看到文件[AD FS 和 Web 應用程式 Proxy SSL 憑證需求](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>AD fs 更換 SSL 憑證
> [!NOTE]
> AD FS SSL 憑證不找到 AD FS 管理嵌入式管理單元 AD FS 服務通訊憑證相同。 若要變更 AD FS SSL 憑證，您將需要使用 PowerShell。

首先，判斷繫結模式的憑證 AD FS 伺服器執行： 預設憑證驗證繫結或其他 client TLS 繫結模式。

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>執行預設的憑證驗證繫結模式 AD fs 更換 SSL 憑證
AD FS 預設執行裝置憑證驗證 443 連接埠和使用者憑證驗證 49443 連接埠 （或不 443 可連接埠）。
在此模式下，使用 powershell cmdlet AdfsSslCertificate 設定的管理 SSL 憑證。

請依照下列步驟：

1. 首先，您必須以取得新的憑證。 這通常是提交憑證簽署要求 (CSR)，第三方、 公用憑證提供者。 有各種不同的方式產生 CSR，包括從 Windows 7 或更高的電腦。 您的供應商應該會有這樣的文件。

    * 請確定憑證符合[AD FS 和 Web 應用程式 Proxy SSL 憑證需求](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. 一旦您取得憑證提供者所提供的回應，請將它匯入到本機存放區每個 AD FS 和 Web 應用程式 Proxy 伺服器上。

1. 在**主要**AD FS 伺服器，使用下列 cmdlet 安裝新的 SSL 憑證

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

您可以找到憑證指紋來執行這個命令：

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>其他資訊

* 設定-AdfsSslCertificate cmdlet 是多節點 cmdlet;這表示它只有執行主要，將更新發電廠中的所有節點。 這是 Server 2016 中的新功能。 Server 2012 R2 上，您必須在每個伺服器上執行設定的 AdfsSslCertificate。
* 設定-AdfsSslCertificate cmdlet 必須執行主要伺服器上。 主要伺服器已執行 Server 2016 和應 2016 引發發電廠行為層級。
* 設定-AdfsSslCertificate cmdlet 將會使用遠端 PowerShell 來設定的其他 AD FS 伺服器，請確認連接埠 5985 (TCP) 是開放式其他節點上。
* 設定-AdfsSslCertificate cmdlet 會授與 adfssrv 主要朗讀的權限的私密金鑰 SSL 憑證。 這個原則代表 AD FS 服務。 您不需要權限授與 AD FS 服務 account 朗讀私密金鑰 SSL 憑證。

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>適用於執行其他 TLS 繫結模式 AD FS 更換 SSL 憑證
在其他 client 設定 TLS 繫結模式，AD FS 執行裝置憑證驗證 443 連接埠與使用者憑證驗證連接埠，443 不同的主機。 主機使用者的憑證會 AD FS 主機附加的 「 certauth 「，例如 「 certauth.fs.contoso.com 」。
在此模式下，使用 powershell cmdlet AdfsAlternateTlsClientBinding 設定的管理 SSL 憑證。 這會管理替代 client TLS 繫結不僅 AD FS 設定 SSL 憑證其他繫結。

請依照下列步驟：

1. 首先，您必須以取得新的憑證。 這通常是提交憑證簽署要求 (CSR)，第三方、 公用憑證提供者。 有各種不同的方式產生 CSR，包括從 Windows 7 或更高的電腦。 您的供應商應該會有這樣的文件。

    * 請確定憑證符合[AD FS 和 Web 應用程式 Proxy SSL 憑證需求](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. 一旦您取得憑證提供者所提供的回應，請將它匯入到本機存放區每個 AD FS 和 Web 應用程式 Proxy 伺服器上。

1. 在**主要**AD FS 伺服器，使用下列 cmdlet 安裝新的 SSL 憑證

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

您可以找到憑證指紋來執行這個命令：

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>其他資訊

* 設定-AdfsAlternateTlsClientBinding cmdlet 是多節點 cmdlet;這表示它只有執行主要，將更新發電廠中的所有節點。
* 設定-AdfsAlternateTlsClientBinding cmdlet 必須執行主要伺服器上。 主要伺服器已執行 Server 2016 和應 2016 引發發電廠行為層級。
* 設定-AdfsAlternateTlsClientBinding cmdlet 將會使用遠端 PowerShell 來設定的其他 AD FS 伺服器，請確認連接埠 5985 (TCP) 是開放式其他節點上。
* 設定-AdfsAlternateTlsClientBinding cmdlet 會授與 adfssrv 主要朗讀的權限的私密金鑰 SSL 憑證。 這個原則代表 AD FS 服務。 您不需要權限授與 AD FS 服務 account 朗讀私密金鑰 SSL 憑證。

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>更換 SSL 憑證 proxy Web 應用程式
我們可以使用設定-WebApplicationProxySslCertificate cmdlet 上 WAP 設定預設憑證驗證繫結或其他 client TLS 繫結模式。
在更換 Web 應用程式 Proxy SSL 憑證，**每個**Web 應用程式的 Proxy 伺服器使用下列 cmdlet 安裝新的 SSL 憑證：

```powershell
Set-WebApplicationProxySslCertificate '<thumbprint of new cert>'
```

如果上述 cmdlet 失敗的原因舊的憑證已經過期，請重新設定使用下列 cmdlet proxy:

```powershell
$cred = Get-Credential
```

輸入是本機系統管理員 AD FS 伺服器上的網域使用者的認證

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>其他參考資料  
* [AD FS 進行驗證憑證的替代主機繫結支援](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [AD FS 和憑證 KeySpec 屬性資訊](../technical-reference/AD-FS-and-KeySpec-Property.md)

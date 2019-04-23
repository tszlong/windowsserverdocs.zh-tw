---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: 管理 Windows Server 2016 中 AD FS 及 WAP 的 SSL 憑證
description: 管理 Windows Server 2016 中 AD FS 及 WAP 的 SSL 憑證
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: abcff8632bc8a3a75af4eee30c3aed046ca0ccc9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877339"
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>管理 Windows Server 2016 中 AD FS 及 WAP 的 SSL 憑證

>適用於：Windows Server 2016

本文說明如何將新的 SSL 憑證部署至您的 AD FS 和 WAP 伺服器。

>[!NOTE]
>取代從現在開始，AD FS 伺服器陣列的 SSL 憑證的建議的方式是使用 Azure AD Connect。  如需詳細資訊，請參閱[更新 Active Directory Federation Services (AD FS) 伺服器陣列的 SSL 憑證](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>取得 SSL 憑證
對於生產 AD FS 伺服器陣列建議使用公開的受信任的 SSL 憑證。 這通常被取得提交憑證簽署要求 (CSR)，第三方、 公開憑證提供者。 有各種不同的方式來產生 CSR，包括從 Windows 7 或更高版本的電腦。 您的供應商應該有這個文件。

- 請確定憑證符合[AD FS 和 Web 應用程式 Proxy 的 SSL 憑證需求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>需要多少憑證
建議您在所有的 AD FS 和 Web 應用程式 Proxy 伺服器之間使用通用的 SSL 憑證。 如需詳細的需求，請參閱文件[AD FS 和 Web 應用程式 Proxy 的 SSL 憑證需求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="ssl-certificate-requirements"></a>SSL 憑證需求
如需包括命名的需求，信任和延伸模組的根查看文件[AD FS 和 Web 應用程式 Proxy 的 SSL 憑證需求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>取代適用於 AD FS 的 SSL 憑證
> [!NOTE]
> AD FS SSL 憑證不是在 AD FS 管理嵌入式管理單元中找到的 AD FS 服務通訊憑證相同。 若要變更 AD FS SSL 憑證，您必須使用 PowerShell。

首先，判斷您的 AD FS 伺服器所執行的憑證繫結模式： 預設憑證驗證繫結或替代的用戶端 TLS 繫結模式。

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>取代預設值執行憑證驗證繫結模式的 AD fs 的 SSL 憑證
連接埠 443 上的裝置憑證驗證和連接埠 49443 上的使用者憑證驗證 （或可設定的連接埠不是 443），則會執行預設的 AD FS。
在此模式中，使用 powershell cmdlet Set-adfssslcertificate 管理 SSL 憑證。

遵循下列步驟：

1. 首先，您必須取得新的憑證。 這通常是由提交憑證簽署要求 (CSR)，第三方、 公開憑證提供者。 有各種不同的方式來產生 CSR，包括從 Windows 7 或更高版本的電腦。 您的供應商應該有這個文件。

    * 請確定憑證符合[AD FS 和 Web 應用程式 Proxy 的 SSL 憑證需求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. 一旦您從您的憑證提供者取得回應，請將它匯入每個 AD FS 和 Web 應用程式 Proxy 伺服器的本機電腦存放區。

1. 在 **主要**AD FS 伺服器上，使用下列 cmdlet 來安裝新的 SSL 憑證

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

可以執行下列命令來找到憑證指紋：

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>其他注意事項

* Set-adfssslcertificate 指令程式是多節點 cmdlet;這表示它只必須從主要節點執行，並將更新伺服器陣列中的所有節點。 這是在 Server 2016 中新功能。 Server 2012 R2 上，您必須在每部伺服器上執行 Set-adfssslcertificate。
* Set-adfssslcertificate cmdlet 不會有只在主要伺服器上執行。 主要伺服器必須正在執行 Server 2016 和伺服陣列行為層級應該引發至 2016年。
* Set-adfssslcertificate cmdlet 會使用 PowerShell 遠端執行功能來設定其他的 AD FS 伺服器，請確定已在其他節點上開啟連接埠 5985 (TCP)。
* Set-adfssslcertificate cmdlet 將 adfssrv 主體讀取權限授與私密金鑰的 SSL 憑證。 此主體表示 AD FS 服務。 您不需要授與 AD FS 服務帳戶讀取權限，SSL 憑證的私密金鑰。

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>取代為在替代的 TLS 繫結模式中執行的 AD FS 的 SSL 憑證
當設定替代的用戶端中的 TLS 繫結模式，AD FS，則會執行連接埠 443 上的裝置憑證驗證和使用者憑證驗證，連接埠 443 上不同的主機名稱上。 使用者憑證主機名稱是 AD FS 的主機名稱之前加上層 」 certauth"，例如"certauth.fs.contoso.com 」。
在此模式中，使用 powershell cmdlet 設定 AdfsAlternateTlsClientBinding 管理 SSL 憑證。 這會管理替代的用戶端 TLS 繫結不僅在其 AD FS 設定的 SSL 憑證的其他所有繫結。

遵循下列步驟：

1. 首先，您必須取得新的憑證。 這通常是由提交憑證簽署要求 (CSR)，第三方、 公開憑證提供者。 有各種不同的方式來產生 CSR，包括從 Windows 7 或更高版本的電腦。 您的供應商應該有這個文件。

    * 請確定憑證符合[AD FS 和 Web 應用程式 Proxy 的 SSL 憑證需求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. 一旦您從您的憑證提供者取得回應，請將它匯入每個 AD FS 和 Web 應用程式 Proxy 伺服器的本機電腦存放區。

1. 在 **主要**AD FS 伺服器上，使用下列 cmdlet 來安裝新的 SSL 憑證

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

可以執行下列命令來找到憑證指紋：

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>其他注意事項

* 此組 AdfsAlternateTlsClientBinding 指令程式是多節點 cmdlet;這表示它只必須從主要節點執行，並將更新伺服器陣列中的所有節點。
* 設定 AdfsAlternateTlsClientBinding cmdlet 不會有只在主要伺服器上執行。 主要伺服器必須正在執行 Server 2016 和伺服陣列行為層級應該引發至 2016年。
* 設定 AdfsAlternateTlsClientBinding cmdlet 會使用 PowerShell 遠端執行功能來設定其他的 AD FS 伺服器，請確定已在其他節點上開啟連接埠 5985 (TCP)。
* 設定 AdfsAlternateTlsClientBinding cmdlet 將 adfssrv 主體讀取權限授與私密金鑰的 SSL 憑證。 此主體表示 AD FS 服務。 您不需要授與 AD FS 服務帳戶讀取權限，SSL 憑證的私密金鑰。

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>取代為 Web 應用程式 Proxy 的 SSL 憑證
WAP 上設定的預設憑證驗證繫結 」 或 「 替代用戶端 TLS 繫結模式中，我們可以使用組 WebApplicationProxySslCertificate cmdlet。
若要取代的 Web 應用程式 Proxy SSL 憑證上,**每個**Web Application Proxy 伺服器會使用下列 cmdlet 來安裝新的 SSL 憑證：

```powershell
Set-WebApplicationProxySslCertificate '<thumbprint of new cert>'
```

如果在上述 cmdlet 失敗，因為舊的憑證已到期，重新設定 proxy，使用下列 cmdlet:

```powershell
$cred = Get-Credential
```

輸入是 AD FS 伺服器上的本機系統管理員的網域使用者的認證

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>其他參考資料  
* [AD FS 支援憑證驗證的替代主機名稱繫結](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [AD FS 和憑證 KeySpec 屬性資訊](../technical-reference/AD-FS-and-KeySpec-Property.md)

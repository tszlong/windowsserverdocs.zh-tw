---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: 管理 Windows Server 2016 中 AD FS 及 WAP 的 SSL 憑證
description: 管理 Windows Server 2016 中 AD FS 及 WAP 的 SSL 憑證
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 230fdaac28f4766c33e62362ca4c7e4d20f22c8e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357744"
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>管理 Windows Server 2016 中 AD FS 及 WAP 的 SSL 憑證



本文說明如何將新的 SSL 憑證部署至您的 AD FS 和 WAP 伺服器。

>[!NOTE]
>取代 AD FS 伺服器陣列的 SSL 憑證的建議方式是使用 Azure AD Connect。  如需詳細資訊，請參閱[更新 Active Directory 同盟服務（AD FS）伺服器陣列的 SSL 憑證](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>取得您的 SSL 憑證
針對生產 AD FS 伺服器陣列，建議使用公開信任的 SSL 憑證。 這通常是藉由將憑證簽署要求（CSR）提交給協力廠商的公開憑證提供者來取得。 有各種方式可產生 CSR，包括從 Windows 7 或更新版本的電腦。 您的廠商應該有此的檔。

- 請確定憑證符合[AD FS 和 Web 應用程式 PROXY SSL 憑證需求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>需要多少憑證
建議您在所有 AD FS 和 Web 應用程式 Proxy 伺服器上使用一般的 SSL 憑證。 如需詳細需求，請參閱檔[AD FS 和 Web 應用程式 PROXY SSL 憑證需求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="ssl-certificate-requirements"></a>SSL 憑證需求
如需相關需求，包括命名、信任的根和延伸模組，請參閱檔[AD FS 和 Web 應用程式 PROXY SSL 憑證需求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>取代 AD FS 的 SSL 憑證
> [!NOTE]
> AD FS SSL 憑證與 AD FS 管理嵌入式管理單元中找到的 AD FS 服務通訊憑證不同。 若要變更 AD FS SSL 憑證，您必須使用 PowerShell。

首先，判斷您的 AD FS 伺服器執行哪種憑證系結模式：預設憑證驗證系結，或替代用戶端 TLS 系結模式。

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>取代以預設憑證驗證系結模式執行之 AD FS 的 SSL 憑證
AD FS 預設會在埠443上執行裝置憑證驗證，並在埠49443上執行使用者憑證驗證（或不是443的可設定通訊埠）。
在此模式中，請使用 powershell Cmdlet Set-Set-adfssslcertificate 來管理 SSL 憑證。

遵循下列步驟：

1. 首先，您將需要取得新的憑證。 這通常是藉由將憑證簽署要求（CSR）提交給協力廠商的公開憑證提供者來完成。 有各種方式可產生 CSR，包括從 Windows 7 或更新版本的電腦。 您的廠商應該有此的檔。

    * 請確定憑證符合[AD FS 和 Web 應用程式 PROXY SSL 憑證需求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. 當您從憑證提供者取得回應之後，請將它匯入到每個 AD FS 和 Web 應用程式 Proxy 伺服器上的本機電腦存放區。

1. 在**主要**AD FS 伺服器上，使用下列 Cmdlet 來安裝新的 SSL 憑證

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

您可以藉由執行下列命令來找到憑證指紋：

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>其他注意事項

* Set-adfssslcertificate Cmdlet 是多節點 Cmdlet;這表示它只需要從主要節點執行，且伺服器陣列中的所有節點都將更新。 這是伺服器2016中的新功能。 在伺服器 2012 R2 上，您必須在每部伺服器上執行 Set-adfssslcertificate 設定。
* Set-adfssslcertificate 指令程式必須只在主伺服器上執行。 主伺服器必須執行伺服器2016，而伺服器陣列行為層級應提升至2016。
* Set-adfssslcertificate 指令會使用 PowerShell 遠端來設定其他 AD FS 伺服器，請確定已在其他節點上開啟埠5985（TCP）。
* Set-adfssslcertificate 指令碼會將 SSL 憑證的私密金鑰讀取權限授與 adfssrv 主體。 此主體代表 AD FS 服務。 不需要將 SSL 憑證之私密金鑰的讀取權授與 AD FS 服務帳戶。

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>取代以替代 TLS 系結模式執行之 AD FS 的 SSL 憑證
在替代用戶端 TLS 系結模式中設定時，AD FS 會在埠443上執行裝置憑證驗證，以及在埠443上的使用者憑證驗證也會在不同的主機名稱。 使用者憑證主機名稱是使用 "certauth" 預先暫止的 AD FS 主機名稱，例如 "certauth.fs.contoso.com"。
在此模式中，請使用 powershell Cmdlet Set-AdfsAlternateTlsClientBinding 來管理 SSL 憑證。 這不僅會管理替代的用戶端 TLS 系結，其他 AD FS 也會設定 SSL 憑證的其他所有系結。

遵循下列步驟：

1. 首先，您將需要取得新的憑證。 這通常是藉由將憑證簽署要求（CSR）提交給協力廠商的公開憑證提供者來完成。 有各種方式可產生 CSR，包括從 Windows 7 或更新版本的電腦。 您的廠商應該有此的檔。

    * 請確定憑證符合[AD FS 和 Web 應用程式 PROXY SSL 憑證需求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. 當您從憑證提供者取得回應之後，請將它匯入到每個 AD FS 和 Web 應用程式 Proxy 伺服器上的本機電腦存放區。

1. 在**主要**AD FS 伺服器上，使用下列 Cmdlet 來安裝新的 SSL 憑證

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

您可以藉由執行下列命令來找到憑證指紋：

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>其他注意事項

* AdfsAlternateTlsClientBinding Cmdlet 是多節點 Cmdlet;這表示它只需要從主要節點執行，且伺服器陣列中的所有節點都將更新。
* AdfsAlternateTlsClientBinding 指令程式必須只在主伺服器上執行。 主伺服器必須執行伺服器2016，而伺服器陣列行為層級應提升至2016。
* AdfsAlternateTlsClientBinding 指令會使用 PowerShell 遠端來設定其他 AD FS 伺服器，請確定已在其他節點上開啟埠5985（TCP）。
* AdfsAlternateTlsClientBinding 指令碼會將 SSL 憑證的私密金鑰讀取權限授與 adfssrv 主體。 此主體代表 AD FS 服務。 不需要將 SSL 憑證之私密金鑰的讀取權授與 AD FS 服務帳戶。

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>取代 Web 應用程式 Proxy 的 SSL 憑證
若要設定 WAP 的預設憑證驗證系結或替代用戶端 TLS 系結模式，我們可以使用 WebApplicationProxySslCertificate 指令程式。
若要取代 Web 應用程式 Proxy SSL 憑證，請在**每個**Web 應用程式 proxy 伺服器上使用下列 Cmdlet 來安裝新的 SSL 憑證：

```powershell
Set-WebApplicationProxySslCertificate -Thumbprint '<thumbprint of new cert>'
```

如果上述 Cmdlet 因為舊憑證已過期而失敗，請使用下列 Cmdlet 重新設定 proxy：

```powershell
$cred = Get-Credential
```

輸入身為 AD FS 伺服器上本機系統管理員之網域使用者的認證

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>其他參考資料  
* [AD FS 支援憑證驗證的替代主機名稱繫結](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [AD FS 和憑證 KeySpec 屬性資訊](../technical-reference/AD-FS-and-KeySpec-Property.md)

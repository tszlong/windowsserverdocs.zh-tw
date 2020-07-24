---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: AD FS 支援憑證驗證的替代主機名稱繫結
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3fd459ac469d052b2c183520cbf434c1ac351c0c
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962750"
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>AD FS 支援憑證驗證的替代主機名稱繫結

在許多網路上，本機防火牆原則可能不允許透過非標準埠（例如49443）的流量。 當您嘗試使用 Windows Server 2016 中 AD FS 之前的 AD FS 完成憑證驗證時，就會發生此問題。 這是因為在相同主機上，您的裝置驗證和使用者憑證驗證不能有不同的系結。 預設通訊埠443系結至接收裝置憑證，而且無法改變以支援相同通道中的多個系結。 結果是智慧卡驗證無法正常執行，而且使用者不知道發生了什麼事，因為根本不會發生任何問題。  
  
透過 Windows Server 2016 中的 AD FS，就可以完成這項作業。
  
在 Windows Server 2016 上的 AD FS 中，這項變更。 現在，我們支援兩種模式，第一個會使用具有不同埠（443、49443）的相同主機（亦即 adfs.contoso.com）。 第二個使用相同埠（443）的不同主機（adfs.contoso.com 和 certauth.adfs.contoso.com）。 這將需要 SSL 憑證，以支援 "certauth" 做為替代主體名稱。 <的 adfs-服務名稱> "。 這可以在建立伺服器陣列時或稍後透過 PowerShell 來完成。  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>如何設定憑證驗證的替代主機名稱系結  
有兩種方式可以新增憑證驗證的替代主機名稱系結。 第一種是設定具有 Windows Server 2016 AD FS 的新 AD FS 伺服器陣列時，如果憑證包含主體別名（SAN），則會自動將它設定為使用上述的第二個方法。 也就是說，它會自動設定兩個不同的主機（sts.contoso.com 和 certauth.sts.contoso.com 具有相同的埠）。 如果憑證不包含 SAN，則您會看到一則警告，告訴您憑證主體別名不支援 certauth. *。 請參閱下列螢幕擷取畫面。 第一個範例顯示憑證具有 SAN 的安裝，第二個則顯示未使用的憑證。  
  
![替代主機名稱系結](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![替代主機名稱系結](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
同樣地，在 Windows Server 2016 中部署 AD FS 之後，您就可以使用 PowerShell Cmdlet： Set-AdfsAlternateTlsClientBinding。
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

出現提示時，按一下 [是] 確認。  這應該是。

![替代主機名稱系結](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>其他參考

* [管理 Windows Server 2016 中 AD FS 及 WAP 的 SSL 憑證](./manage-ssl-certificates-ad-fs-wap.md)

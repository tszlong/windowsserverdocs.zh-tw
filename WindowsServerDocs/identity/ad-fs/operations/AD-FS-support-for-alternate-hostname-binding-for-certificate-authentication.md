---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: AD FS 支援憑證驗證的替代主機名稱繫結
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: f5e38ad5b3bcf81774ae5d6ae8908d5103455385
ms.sourcegitcommit: b47bed17c2b1c6b61e904b8deb374e2c60e692e4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2020
ms.locfileid: "96526075"
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>AD FS 支援憑證驗證的替代主機名稱繫結

在許多網路中，本機防火牆原則可能不允許透過非標準埠（例如49443）的流量。 當您在 Windows Server 2016 中 AD FS 之前，嘗試使用 AD FS 完成憑證驗證時，就會發生這種問題。 這是因為您在同一部主機上無法使用不同的裝置驗證系結和使用者憑證驗證。 預設通訊埠443系結至接收裝置憑證，無法更改以支援相同通道中的多個系結。 這是因為智慧卡驗證無法運作，而且使用者不知道發生什麼事，因為並不會指出真正發生什麼事。

有了 Windows Server 2016 中的 AD FS，就可以完成這項作業。

在 Windows Server 2016 上的 AD FS 中，這已變更。 現在，我們支援兩種模式，第一種是使用相同的主機 (也就是使用不同埠的 adfs.contoso.com)  (443、49443) 。 第二個使用不同主機 (adfs.contoso.com 和 certauth.adfs.contoso.com) ，其埠 (443) 。 這需要 SSL 憑證，才能支援 "certauth" <adfs-服務名稱> "作為替代主體名稱。 這可以在建立伺服器陣列時或稍後透過 PowerShell 來完成。

## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>如何針對憑證驗證設定替代主機名稱系結
您可以透過兩種方式來新增憑證驗證的替代主機名稱系結。 第一個方法是使用 Windows Server 2016 的 AD FS 來設定新的 AD FS 伺服器陣列，如果憑證包含 (SAN) 的主體別名，則會自動將它設定為使用上述的第二個方法。 也就是說，它會自動設定兩部不同的主機 (sts.contoso.com 和 certauth.sts.contoso.com，並使用相同的埠。 如果憑證未包含 SAN，您將會看到一則警告，指出憑證主體別名不支援 certauth. *。 請參閱下列螢幕擷取畫面。 第一個顯示的是憑證有 SAN 的安裝，第二個則顯示未安裝的憑證。

![替代主機名稱系結](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)

![替代主機名稱系結](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)

同樣地，在部署 Windows Server 2016 中的 AD FS 之後，您就可以使用 PowerShell Cmdlet： AdfsAlternateTlsClientBinding。

```powershell
Set-AdfsAlternateTlsClientBinding -Member ADFS1.contoso.com -Thumbprint '<thumbprint of cert>'
```

出現提示時，請按一下 [是] 以確認。  這應該是如此。

![替代主機名稱系結](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>其他參考資料

* [管理 Windows Server 2016 中 AD FS 及 WAP 的 SSL 憑證](./manage-ssl-certificates-ad-fs-wap.md)

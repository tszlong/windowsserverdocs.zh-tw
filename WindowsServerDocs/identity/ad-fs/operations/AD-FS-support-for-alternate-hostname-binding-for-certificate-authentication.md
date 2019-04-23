---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: AD FS 支援憑證驗證的替代主機名稱繫結
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 553ff059693c7b0c0e6f0364d82c1adbca661097
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887249"
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>AD FS 支援憑證驗證的替代主機名稱繫結

>適用於：Windows Server 2016

在許多網路上的本機防火牆原則可能不允許流量透過非標準連接埠 49443 等。 嘗試完成之前 Windows Server 2016 中的 AD FS 的 AD FS 使用的憑證驗證時，這會成為問題。 這是因為您不能有裝置驗證和使用者憑證驗證的不同繫結到相同主機上。 預設連接埠 443 繫結至接收裝置憑證，並無法改變以支援在相同的通道中的多個繫結。 結果是，智慧卡驗證就無法運作，而且使用者已知道因為沒有真正發生的指示，發生了什麼事。  
  
Windows Server 2016 中的 AD FS 即可。
  
在 Windows Server 2016 上的 AD FS 中，這已變更。 我們現在支援兩種模式，則第一個會將相同的主機 (也就是 adfs.contoso.com) 使用不同的連接埠 （443、 49443）。 第二個使用不同的主控件 （adfs.contoso.com 和 certauth.adfs.contoso.com） 使用相同的連接埠 (443)。 這將需要 SSL 憑證來支援 「 certauth。 < adfs--s e n > 做為替代主體名稱。 這可以在建立陣列時或稍後透過 PowerShell 完成。  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>如何設定憑證驗證的替代主機名稱繫結  
有兩種方式，您可以新增憑證驗證的替代主機名稱繫結。 首先，設定新的 AD FS 伺服器陣列與 AD FS，適用於 Windows Server 2016 中，如果憑證包含主體別名 (SAN)，則系統會自動將安裝程式以使用先前所述的第二個方法時。 也就是說，它會自動設定兩個不同的主機 （sts.contoso.com 和 certauth.sts.contoso.com 的相同連接埠。 如果憑證沒有 SAN，您會看到警告，指出憑證主體替代名稱中不支援 certauth.*。 請參閱以下螢幕擷取畫面。 第一個顯示其中的憑證有 SAN，而第二個示範未將憑證安裝。  
  
![替代的主機名稱繫結](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![替代的主機名稱繫結](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
同樣地，Windows Server 2016 中的 AD FS 部署之後，您可以使用 PowerShell cmdlet:Set-AdfsAlternateTlsClientBinding.
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

出現提示時，按一下 [是] 確認。  而且，應該是它。

![替代的主機名稱繫結](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>其他參考資料

* [在 AD FS 和 Windows Server 2016 中的 WAP 中管理 SSL 憑證](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: "AD FS 進行驗證憑證的替代主機繫結支援"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 553ff059693c7b0c0e6f0364d82c1adbca661097
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>AD FS 進行驗證憑證的替代主機繫結支援

>適用於：Windows Server 2016

在許多網路上的本機防火牆原則可能不允許流量透過 49443 像的標準連接埠。 這嘗試以完成憑證驗證，AD FS 之前的 Windows Server 2016 AD FS 使用時變得的問題。 這是因為您未可能會有不同裝置驗證及使用者憑證的驗證連結同一部主機上。 預設的連接埠 443 繫結至接收的裝置上的憑證，並無法以相同的通道支援多繫結變更。 結果已智慧卡驗證無法運作，是因為不會顯示很事情所要發生的問題，您不知道的使用者。  
  
AD FS 在 Windows Server 2016 的作法。
  
在 Windows Server 2016 上 AD FS 這已變更。 我們支援兩種模式，現在第一個會使用不同的連接埠 （443、 49443） 相同的主機 (亦即 adfs.contoso.com)。 第二個不同的主機 （adfs.contoso.com 和 certauth.adfs.contoso.com） 使用相同的連接埠 (443)。 這將會需要支援 「 certauth。 < adfs 服務-名稱 > 」 做為備用主體名稱 SSL 憑證。 這可以稍後透過 PowerShell 發電廠建立的時間。  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>如何設定替代主機驗證憑證的名稱繫結  
有兩種方式，您可以將其他主機驗證憑證的名稱繫結新增。 首先，如果憑證包含主題替代名稱 （舊），則它會自動設定為使用上述的第二個方法設定新的 Windows Server 2016 AD FS 使用 AD FS 發電廠時。 是的它將會自動設定兩個不同的主機 （sts.contoso.com 和 certauth.sts.contoso.com 使用相同的連接埠。 如果憑證不包含舊，您會看到一則警告憑證主題替代名稱不支援 certauth.* 告知您。 查看下的螢幕擷取畫面。 第一個顯示位置憑證有舊，會顯示憑證不第二個安裝。  
  
![替代主機繫結](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![替代主機繫結](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
同樣地，在已部署 Windows Server 2016 中的 AD FS 之後您可以使用 PowerShell cmdlet： 設定-AdfsAlternateTlsClientBinding。
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

出現提示時，按一下 [是] 進行確認。  而且，應該它。

![替代主機繫結](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>其他參考資料

* [AD FS 和 Windows Server 2016 中的 WAP 管理 SSL 憑證](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

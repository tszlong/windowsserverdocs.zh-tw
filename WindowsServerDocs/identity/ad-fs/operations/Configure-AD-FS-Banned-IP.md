---
title: 設定 AD FS 禁用的 IP 位址
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/28/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 01ef992554a1e0961d8d795e9baa7730a1a1d682
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189895"
---
# <a name="ad-fs-and-banned-ip-addresses"></a>AD FS 和遭到禁用的 IP 位址


在 2018 年 6 月，在 Windows Server 2016 上的 AD FS 引進了**禁用 Ip**與 AD FS 2018 年 6 月更新。  此更新可讓您在 AD FS 中，全域設定一組 IP 位址，讓來自這些 IP 位址，或要求中有這些 IP 位址**x 轉送 for**或**x-ms-轉送-用戶端-ip**標頭，將會封鎖 AD fs。

## <a name="adding-banned-ips"></a>新增帳戶已被禁用的 Ip
若要加入的全域清單遭到禁用的 Ip，請使用下列 Powershell cmdlet:

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

允許的格式

1.  IPv4
2.  IPv6
3.  使用 IPv4 或 v6 CIDR 格式

沒有限制為 300 個項目遭到禁用的 IP 位址。 您可以使用 CIDR 或範圍的格式來拒絕大型區塊的單一項目使用的項目。

## <a name="removing-banned-ips"></a>移除禁止 Ip
若要移除的全域清單遭到禁用的 Ip，請使用下列 Powershell cmdlet:

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>讀取禁用 Ip
若要讀取目前的遭到禁用的 IP 位址集，使用下列 Powershell cmdlet:

``` powershell
PS C:\ >Get-AdfsProperties 
```

輸出範例：

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>其他參考資料  
[保護 Active Directory Federation Services 的最佳做法](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)

---
title: 設定 AD FS 的禁用 IP 位址
description: 深入瞭解： AD FS 和禁止的 IP 位址
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/28/2018
ms.topic: article
ms.openlocfilehash: 3366088579f3a78dd818ac1cef6bb8894ffe5f57
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040196"
---
# <a name="ad-fs-and-banned-ip-addresses"></a>AD FS 和禁止的 IP 位址


在2018年6月，Windows Server 2016 上的 AD FS 引進了 AD FS 6 月2018更新的 **禁用 ip** 。  這項更新可讓您在 AD FS 中全域設定一組 IP 位址，如此一來，來自這些 IP 位址的要求，或在 **x 轉送** 的或 **x 毫秒轉送的用戶端 ip** 標頭中具有這些 ip 位址的要求，將會被 AD FS 封鎖。

## <a name="adding-banned-ips"></a>新增禁用的 Ip
若要將禁用的 Ip 新增至全域清單，請使用下列 Powershell Cmdlet：

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

允許的格式

1.  IPv4
2.  IPv6
3.  使用 IPv4 或 v6 的 CIDR 格式

針對禁用的 IP 位址，有300個專案的限制。 您可以使用 CIDR 或範圍格式來拒絕具有單一專案的大型專案區塊。

## <a name="removing-banned-ips"></a>移除禁用的 Ip
若要從全域清單中移除禁用的 Ip，請使用下列 Powershell Cmdlet：

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>讀取禁用的 Ip
若要讀取目前的禁用 IP 位址集，請使用下列 Powershell Cmdlet：

``` powershell
PS C:\ >Get-AdfsProperties
```

範例輸出︰

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>其他參考資料
[保護 Active Directory 同盟服務的最佳作法](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[設定-Set-adfsproperties](/powershell/module/adfs/set-adfsproperties)

[AD FS 操作](../ad-fs-operations.md)

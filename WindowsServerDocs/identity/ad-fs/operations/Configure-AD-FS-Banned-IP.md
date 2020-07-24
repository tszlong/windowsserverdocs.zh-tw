---
title: 設定 AD FS 禁止的 IP 位址
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/28/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9e38105bafc92efc4d9e62e4815cdb24c3c25512
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965610"
---
# <a name="ad-fs-and-banned-ip-addresses"></a>AD FS 和禁用的 IP 位址


在2018年6月，Windows Server 2016 上的 AD FS 引進了已**禁用的 ip** ，其 AD FS 6 月2018更新。  此更新可讓您在 AD FS 中，全域設定一組 IP 位址，因此來自那些 IP 位址的要求，或在**x 轉送**的-----轉送------**轉寄的用戶端 ip**標頭中有那些 ip 位址，將會被 AD FS 封鎖。

## <a name="adding-banned-ips"></a>新增禁用的 Ip
若要將已禁用的 Ip 新增至全域清單，請使用下列 Powershell Cmdlet：

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

允許的格式

1.  IPv4
2.  IPv6
3.  使用 IPv4 或 v6 的 CIDR 格式

禁止的 IP 位址有300個專案的限制。 您可以使用 CIDR 或範圍格式來拒絕含有單一專案的大型專案區塊。

## <a name="removing-banned-ips"></a>移除禁用的 Ip
若要從全域清單中移除已禁用的 Ip，請使用下列 Powershell Cmdlet：

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>讀取禁止的 Ip
若要讀取目前已禁用的 IP 位址組，請使用下列 Powershell Cmdlet：

``` powershell
PS C:\ >Get-AdfsProperties 
```

範例輸出︰

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>其他參考  
[保護 Active Directory 同盟服務的最佳做法](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[設定-Set-adfsproperties](/powershell/module/adfs/set-adfsproperties?view=win10-ps)

[AD FS 操作](../ad-fs-operations.md)

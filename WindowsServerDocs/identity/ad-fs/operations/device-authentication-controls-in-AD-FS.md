---
title: AD FS 中的裝置驗證控制項
description: 本檔說明如何在 Windows Server 2016 和 2012 R2 的 AD FS 中啟用裝置驗證
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f9cc811c6a78e58ff3550343e89c19806b9914fb
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966870"
---
# <a name="device-authentication-controls-in-ad-fs"></a>AD FS 中的裝置驗證控制項
下列檔說明如何在 Windows Server 2016 和 2012 R2 中啟用裝置驗證控制項。

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>AD FS 2012 R2 中的裝置驗證控制
原本在 AD FS 2012 R2 中，有一個名為的全域驗證屬性 `DeviceAuthenticationEnabled` ，其受控制的裝置驗證。

若要進行此設定，請 `Set-AdfsGlobalAuthenticationPolicy` 使用 Cmdlet，如下所示：


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



若要停用裝置驗證，則會使用相同的 Cmdlet 將值設定為 $false。

## <a name="device-authentication-controls-in-ad-fs-2016"></a>AD FS 2016 中的裝置驗證控制項
2012 R2 中唯一支援的裝置驗證類型是 Clienttls 是。  在 AD FS 2016 中，除了 Clienttls 是，還有兩種新類型的裝置驗證適用于新式裝置驗證。  這些節點為：
- PKeyAuth
- PRT

為了控制新的行為，此 `DeviceAuthenticationEnabled` 屬性會搭配名為的新屬性一起使用 `DeviceAuthenticationMethod` 。  

裝置驗證方法會決定要完成的裝置驗證類型： PRT、PKeyAuth、Clienttls 是或某種組合。
其值如下：
 - SignedToken：僅 PRT
 - PKeyAuth： PRT + PKeyAuth
 - Clienttls 是： PRT + Clienttls 是
 - 全部：以上皆是

如您所見，PRT 是所有裝置驗證方法的一部分，使其作用於設定為時一律啟用的預設方法 `DeviceAuthenticationEnabled` `$true` 。

範例：若要設定方法，請使用上述的 DeviceAuthenticationEnabled Cmdlet，連同新的屬性：

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```

>[!NOTE]
> 在 ADFS 2019 中， `DeviceAuthenticationMethod` 可以搭配命令使用 `Set-AdfsRelyingPartyTrust` 。

``` powershell
PS:\>Set-AdfsRelyingPartyTrust -DeviceAuthenticationMethod ClientTLS
```

>[!NOTE]
> 啟用裝置驗證（將設定 `DeviceAuthenticationEnabled` 為 `$true` ）表示 `DeviceAuthenticationMethod` 會隱含地設定為 `SignedToken` ，這等同于**PRT**。


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
> [!NOTE]
> 預設的裝置驗證方法是 `SignedToken` 。  其他值為**PKeyAuth、**<strong>clienttls 是</strong>和**All**。

`DeviceAuthenticationMethod`從 AD FS 2016 發行以來，值的意義稍有變更。  根據更新層級而定，請參閱下表中每個值的意義：


|AD FS 版本|DeviceAuthenticationMethod 值|利用|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|PRT + PkeyAuth|
||Clienttls 是|Clienttls 是|
||全部|PRT + PkeyAuth + Clienttls 是|
|2016 RTM + Windows Update 的最新狀態|SignedToken （已變更的意義）|PRT （僅限）|
||PkeyAuth （新增）|PRT + PkeyAuth|
||Clienttls 是|PRT + Clienttls 是|
||全部|PRT + PkeyAuth + Clienttls 是|

## <a name="see-also"></a>另請參閱
[AD FS 操作](../ad-fs-operations.md)

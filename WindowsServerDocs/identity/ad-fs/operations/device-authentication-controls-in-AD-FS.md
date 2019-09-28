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
ms.openlocfilehash: 87c011b18ad4a1d464072c1ea90b09a44e831378
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407361"
---
# <a name="device-authentication-controls-in-ad-fs"></a>AD FS 中的裝置驗證控制項
下列檔說明如何在 Windows Server 2016 和 2012 R2 中啟用裝置驗證控制項。

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>AD FS 2012 R2 中的裝置驗證控制
原本在 AD FS 2012 R2 中，有一個稱為 @no__t 的全域驗證屬性，可控制裝置驗證。

若要設定，請使用 `Set-AdfsGlobalAuthenticationPolicy` Cmdlet，如下所示：


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



若要停用裝置驗證，則會使用相同的 Cmdlet 將值設定為 $false。

## <a name="device-authentication-controls-in-ad-fs-2016"></a>AD FS 2016 中的裝置驗證控制項
2012 R2 中唯一支援的裝置驗證類型是 Clienttls 是。  在 AD FS 2016 中，除了 Clienttls 是，還有兩種新類型的裝置驗證適用于新式裝置驗證。  這些是：
- PKeyAuth
- PRT

為了控制新的行為，`DeviceAuthenticationEnabled` 屬性會與稱為 `DeviceAuthenticationMethod` 的新屬性搭配使用。  

裝置驗證方法會決定要執行的裝置驗證類型：PRT、PKeyAuth、Clienttls 是或某種組合。
其值如下：
 - SignedToken:僅限 PRT
 - PKeyAuth:PRT + PKeyAuth
 - Clienttls 是PRT + Clienttls 是
 - 所有：上述所有檔案

如您所見，PRT 是所有裝置驗證方法的一部分，使其生效 `DeviceAuthenticationEnabled` 設定為 `$true` 時，一律會啟用的預設方法。

範例：若要設定方法，請使用上述的 DeviceAuthenticationEnabled Cmdlet 以及新的屬性：

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```

>[!NOTE]
> 在 ADFS 2019 中，`DeviceAuthenticationMethod` 可以搭配 `Set-AdfsRelyingPartyTrust` 命令使用。

``` powershell
PS:\>Set-AdfsRelyingPartyTrust -DeviceAuthenticationMethod ClientTLS
```

>[!NOTE]
> 啟用裝置驗證（將 `DeviceAuthenticationEnabled` 設定為 `$true`）表示 `DeviceAuthenticationMethod` 會隱含設定為 `SignedToken`，這等同于**PRT**。


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
> [!NOTE]
> 預設的裝置驗證方法為 `SignedToken`。  其他值為**PKeyAuth、** <strong>clienttls 是</strong>和**All**。

AD FS 2016 發行後，@no__t 值的意義已稍微變更。  根據更新層級而定，請參閱下表中每個值的意義：


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
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)

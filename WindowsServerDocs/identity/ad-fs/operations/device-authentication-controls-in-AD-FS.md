---
title: 在 AD FS 中的裝置驗證控制項
description: 本文件說明如何啟用適用於 Windows Server 2016 和 2012 R2 的 AD FS 中的裝置驗證
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d66cfde20060229844c34abeea85dd83b802ddad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822819"
---
# <a name="device-authentication-controls-in-ad-fs"></a>在 AD FS 中的裝置驗證控制項
下列文件會示範如何啟用 Windows Server 2016 和 2012 R2 中的裝置驗證控制項。

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>在 AD FS 2012 R2 中的裝置驗證控制項
原本在 AD FS 2012 R2 時發生一個通用的驗證屬性名`DeviceAuthenticationEnabled`受控制的裝置驗證。

若要設定的設定，`Set-AdfsGlobalAuthenticationPolicy`指令程式用，如下所示：


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



若要停用裝置驗證，相同的 cmdlet 用來將值設為 $false。

## <a name="device-authentication-controls-in-ad-fs-2016"></a>在 AD FS 2016 中的裝置驗證控制項
2012 R2 中支援的裝置驗證的唯一類型的 clientTLS。  在 AD FS 2016 中，除了 clientTLS 有兩個新類型的新式裝置驗證的裝置驗證。  這些是：
- PKeyAuth
- PRT

若要控制新的行為，`DeviceAuthenticationEnabled`屬性與搭配使用新的屬性，稱為`DeviceAuthenticationMethod`。  

裝置的驗證方法會決定將完成的裝置驗證的類型：PRT PKeyAuth、 clientTLS 或某種組合。
它有下列值：
 - SignedToken:只有 PRT
 - PKeyAuth:PRT + PKeyAuth
 - ClientTLS:PRT + clientTLS 
 - 所有：上述所有檔案

如您所見，PRT 是一部分的所有裝置的驗證方法，讓作用中的預設方法永遠啟用`DeviceAuthenticationEnabled`設為`$true`。

範例：若要設定的方法，使用 DeviceAuthenticationEnabled cmdlet 為以上版本，以及新的屬性：

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```
>[!NOTE]
> 啟用裝置驗證 (設定`DeviceAuthenticationEnabled`要`$true`) 表示`DeviceAuthenticationMethod`隱含地設定為`SignedToken`，這等同於**PRT**。


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
>[!NOTE]
>預設裝置驗證方法是`SignedToken`。  其他的值為**PKeyAuth，* * * ClientTLS，** 並**所有**。

意義`DeviceAuthenticationMethod`值已稍微變更，因為 AD FS 2016 已發行。  請參閱下表根據更新層級的每個值的意義：


|AD FS 版本|DeviceAuthenticationMethod 值|方法|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|PRT + PkeyAuth|
||clientTLS|clientTLS|
||全部|PRT PkeyAuth + clientTLS|
|2016 RTM + 最新 Windows update|SignedToken （亦即已變更）|PRT （僅限）|
||PkeyAuth （新）|PRT + PkeyAuth|
||clientTLS|PRT + clientTLS|
||全部|PRT PkeyAuth + clientTLS|

## <a name="see-also"></a>另請參閱
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)

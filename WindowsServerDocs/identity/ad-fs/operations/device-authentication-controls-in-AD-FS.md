---
title: "AD FS 裝置驗證控制項"
description: "告訴您如何讓裝置驗證，AD FS 適用於 Windows Server 2016 和 2012 R2 中的，這份文件"
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d66cfde20060229844c34abeea85dd83b802ddad
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="device-authentication-controls-in-ad-fs"></a>AD FS 裝置驗證控制項
下列文件示範如何讓 Windows Server 2016 和 2012 R2 裝置驗證控制項。

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>AD FS 2012 R2 裝置驗證控制項
原始中 AD FS 2012 R2 是一個通用驗證屬性名為`DeviceAuthenticationEnabled`裝置驗證。

若要設定的設定，請`Set-AdfsGlobalAuthenticationPolicy`cmdlet 使用，如下所示：


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



若要停用裝置驗證，相同 cmdlet 將值設定為 $false 使用。

## <a name="device-authentication-controls-in-ad-fs-2016"></a>AD FS 2016 裝置驗證控制項
僅限類型的裝置驗證 2012 R2 的支援已 clientTLS。  在 AD FS 2016，除了 clientTLS 有兩種新裝置進行驗證現代裝置的驗證。  以下是：
- PKeyAuth
- PRT

若要控制新的行為，`DeviceAuthenticationEnabled`屬性搭配搭配新的屬性稱為「`DeviceAuthenticationMethod`。  

裝置的驗證方法判斷裝置驗證，將會完成類型：PRT、PKeyAuth，clientTLS，或部分組合。
它會有下列值：
 - 僅限 SignedToken: PRT
 - PKeyAuth: PRT + PKeyAuth
 - ClientTLS: PRT + clientTLS 
 - 所有：以上皆

您可以看到 PRT 屬於的所有裝置的驗證方法，讓您做的變更預設的方法，都時支援`DeviceAuthenticationEnabled`設定為 [ `$true`。

範例：若要設定的方法，請使用 DeviceAuthenticationEnabled cmdlet 為上述，以及新的屬性：

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```
>[!NOTE]
> 啟用裝置驗證 (設定`DeviceAuthenticationEnabled`到`$true`) 表示`DeviceAuthenticationMethod`隱含設為`SignedToken`，相當於**PRT**。


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
>[!NOTE]
>預設裝置驗證方法是`SignedToken`。  其他的值為**PKeyAuth，* * * ClientTLS，**和**所有**。

意義`DeviceAuthenticationMethod`AD FS 2016 發行後稍微變更值。  查看下表中的每一個值，根據更新層級的意義︰


|AD FS 版本|DeviceAuthenticationMethod 值。|表示|
| ----- | ----- | ----- |
|RTM 2016|SignedToken|PRT + PkeyAuth|
||clientTLS|clientTLS|
||所有|PRT + PkeyAuth + clientTLS|
|RTM 2016 + 取得最新的 Windows 更新|SignedToken（變更意義）|（僅限）PRT|
||PkeyAuth（新增）|PRT + PkeyAuth|
||clientTLS|PRT + clientTLS|
||所有|PRT + PkeyAuth + clientTLS|

## <a name="see-also"></a>也了
[AD FS 作業](../../ad-fs/AD-FS-2016-Operations.md)

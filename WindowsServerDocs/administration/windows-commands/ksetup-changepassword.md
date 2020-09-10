---
title: ksetup changepassword
description: '>ksetup changepassword 命令的參考文章，使用金鑰發佈中心 (KDC) 密碼 (kpasswd) 值來變更已登入使用者的密碼。'
ms.topic: reference
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ff88136c51b9790867c76d54467996e9305cbc93
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639715"
---
# <a name="ksetup-changepassword"></a>ksetup changepassword

使用金鑰發佈中心 (KDC) 密碼 (kpasswd) 值來變更已登入使用者的密碼。 命令的輸出會通知您「成功」或「失敗」狀態。

您可以藉由執行**kpasswd** `ksetup /dumpstate` 命令並查看輸出來檢查是否已設定 kpasswd。


## <a name="syntax"></a>語法

```
ksetup /changepassword <oldpassword> <newpassword>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<oldpassword>` | 指定已登入使用者的現有密碼。 |
| `<newpassword>` | 指定登入使用者的新密碼。 此密碼必須符合在這部電腦上設定的所有密碼需求。 |

#### <a name="remarks"></a>備註

- 如果在目前的網域中找不到使用者帳戶，系統會要求您提供使用者帳戶所在的功能變數名稱。

- 如果您想要在下次登入時強制密碼變更，此命令允許使用星號 ( * ) ，因此系統會提示使用者輸入新密碼。

-

### <a name="examples"></a>範例

若要變更目前登入此網域中這部電腦之使用者的密碼，請輸入：

```
ksetup /changepassword Pas$w0rd Pa$$w0rd
```

若要變更目前在 Contoso 網域中登入之使用者的密碼，請輸入：

```
ksetup /domain CONTOSO /changepassword Pas$w0rd Pa$$w0rd
```

若要強制目前登入的使用者在下次登入時變更密碼，請輸入：

```
ksetup /changepassword Pas$w0rd *
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [>ksetup 命令](ksetup.md)

- [>ksetup dumpstate 命令](ksetup-dumpstate.md)

- [>ksetup addkpasswd 命令](ksetup-addkpasswd.md)

- [>ksetup delkpasswd 命令](ksetup-delkpasswd.md)

- [>ksetup dumpstate 命令](ksetup-dumpstate.md)

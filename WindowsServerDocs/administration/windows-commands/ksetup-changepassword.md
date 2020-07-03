---
title: ksetup changepassword
description: Ksetup changepassword 命令的參考文章，它會使用金鑰發佈中心（KDC）密碼（kpasswd）值來變更已登入使用者的密碼。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e49a9c0a796357c89efd3c86373c77468670176c
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925509"
---
# <a name="ksetup-changepassword"></a>ksetup changepassword

會使用金鑰發佈中心（KDC）密碼（kpasswd）值來變更已登入使用者的密碼。 命令的輸出會通知您「成功」或「失敗」狀態。

您可以藉由執行**kpasswd** `ksetup /dumpstate` 命令並查看輸出，來檢查是否已設定 kpasswd。


## <a name="syntax"></a>語法

```
ksetup /changepassword <oldpassword> <newpassword>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<oldpassword>` | 指定登入使用者的現有密碼。 |
| `<newpassword>` | 指定登入使用者的新密碼。 此密碼必須符合在此電腦上設定的所有密碼需求。 |

#### <a name="remarks"></a>備註

- 如果在目前的網域中找不到使用者帳戶，系統會要求您提供使用者帳戶所在的功能變數名稱。

- 如果您想要在下次登入時強制變更密碼，此命令允許使用星號（*），因此系統會提示使用者輸入新密碼。

-

### <a name="examples"></a>範例

若要變更目前在此網域中登入這部電腦之使用者的密碼，請輸入：

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

- [ksetup 命令](ksetup.md)

- [ksetup dumpstate 命令](ksetup-dumpstate.md)

- [ksetup addkpasswd 命令](ksetup-addkpasswd.md)

- [ksetup delkpasswd 命令](ksetup-delkpasswd.md)

- [ksetup dumpstate 命令](ksetup-dumpstate.md)

---
title: subst
description: Subst 命令的參考文章，此命令會將路徑與磁碟機號產生關聯。
ms.topic: reference
ms.assetid: 3e69234c-2312-4343-868b-afc1017c622a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 52d45c9139c70c4f513a972f7789f872145c8b63
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718225"
---
# <a name="subst"></a>subst

將路徑與磁碟機號產生關聯。 如果使用時不含參數，則 **subst** 會顯示作用中虛擬磁片磁碟機的名稱。

## <a name="syntax"></a>語法

```
subst [<drive1>: [<drive2>:]<path>]
subst <drive1>: /d
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<drive1>:` | 指定要指派路徑的虛擬磁片磁碟機。 |
| `[<drive2>:]<path>` | 指定要指派給虛擬磁片磁碟機的實體磁片磁碟機和路徑。 |
| /d | 刪除替代的 (虛擬) 磁片磁碟機。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

- 下列命令無法運作，且不得用於 **subst** 命令中指定的磁片磁碟機：

  - [chkdsk 命令](chkdsk.md)

    [diskcomp 命令](diskcomp.md)

    [diskcopy 命令](diskcopy.md)

    [format 命令](format.md)

    [標籤命令](label.md)

    [復原命令](recover.md)

- `<drive1>`參數必須在**lastdrive**命令指定的範圍內。 如果沒有，則 **subst** 會顯示下列錯誤訊息： `Invalid parameter - drive1:`

## <a name="examples"></a>範例

若要建立路徑 b:\user\betty\forms 的虛擬磁片磁碟機 z，請輸入：

```
subst z: b:\user\betty\forms
```

您可以輸入虛擬磁片磁碟機的字母，並在後面加上冒號，而不是輸入完整路徑，如下所示：

```
z:
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
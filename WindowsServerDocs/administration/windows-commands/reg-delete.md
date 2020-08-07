---
title: reg delete
description: Reg delete 命令的參考文章，它會刪除登錄中的子機碼或專案。
ms.topic: article
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da541f55117e287df81b53a45c923ed2ed3ae028
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884151"
---
# <a name="reg-delete"></a>reg delete

從登錄中刪除子機碼或專案。

## <a name="syntax"></a>語法

```
reg delete <keyname> [{/v Valuename | /ve | /va}] [/f]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<keyname1>` | 指定要新增之子機碼或專案的完整路徑。 若要指定遠端電腦，請將電腦名稱稱的格式加上 `\\<computername>\`) 作為*keyname*的一部分 (。 省略 `\\<computername>\` 會使操作預設為本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和**HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM**和**HKU**。 如果登錄機碼名稱包含空格，請用引號括住機碼名稱。 |
| 停`<Valuename>` | 刪除子機碼下的特定專案。 如果未指定任何專案，則會刪除子機碼下的所有專案和子機碼。 |
| /ve | 指定只會刪除沒有值的專案。 |
| /va | 刪除指定子機碼下的所有專案。 不會刪除指定子機碼下的子機碼。 |
| /f | 刪除現有的登錄子機碼或專案，而不要求確認。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Reg delete**作業的傳回值如下：

    | 值 | 描述 |
    |--|--|
    | 0 | 成功 |
    | 1 | 失敗 |

### <a name="examples"></a>範例

若要刪除登錄機碼的 Timeout 和其所有子機碼和值，請輸入：

```
reg delete HKLM\Software\MyCo\MyApp\Timeout
```

若要在名為天干//的電腦上刪除 HKLM\Software\MyCo 下的登錄值 MTU，請輸入：

```
reg delete \\ZODIAC\HKLM\Software\MyCo /v MTU
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

---
title: bitsadmin addfileset
description: Bitsadmin addfileset 命令的參考文章，它會將一或多個檔案加入至指定的作業。
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52a97817bd734a06ba787cb6faf17f2a03419da8
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894912"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

將一或多個檔案加入至指定的作業。

## <a name="syntax"></a>語法

```
bitsadmin /addfileset <job> <textfile>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| textfile | 文字檔，每一行包含遠端和本機檔案名。 **注意：** 名稱必須以空格分隔。 以字元開頭的行 `#` 會被視為批註。 |

## <a name="examples"></a>範例

```
bitsadmin /addfileset files.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

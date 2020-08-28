---
title: bitsadmin addfileset
description: Bitsadmin addfileset 命令的參考文章，可將一或多個檔案加入至指定的作業。
ms.topic: reference
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2b9b93f38f3604c4f0a9fcaf886d74356d355086
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033706"
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
| textfile | 文字檔，每一行都包含遠端和本機檔案名。 **注意：** 名稱必須以空格分隔。 以字元開頭的行 `#` 會被視為批註。 |

## <a name="examples"></a>範例

```
bitsadmin /addfileset files.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

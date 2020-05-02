---
title: bitsadmin addfileset
description: Bitsadmin addfileset 命令的參考主題，它會將一或多個檔案加入至指定的作業。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d610c1330818cf820923b6d4f2e3555dc477444b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718473"
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
| textfile | 文字檔，每一行包含遠端和本機檔案名。 **注意：** 名稱必須以空格分隔。 以`#`字元開頭的行會被視為批註。 |

## <a name="examples"></a>範例

```
bitsadmin /addfileset files.txt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

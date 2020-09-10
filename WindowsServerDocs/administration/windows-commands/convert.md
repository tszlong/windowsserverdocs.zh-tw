---
title: convert
description: Convert 命令的參考文章，可將磁片從一個磁片類型轉換成另一個磁片類型。
ms.topic: reference
ms.assetid: ae151297-af21-4701-bd69-21d775518e03
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 654d5266fff3a3fffadb9d176eb07a792f22fa01
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629279"
---
# <a name="convert"></a>convert

將磁片從一種磁片類型轉換成另一種。

## <a name="syntax"></a>語法

```
convert basic
convert dynamic
convert gpt
convert mbr
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| [轉換基本命令](convert-basic.md) | 將空的動態磁碟轉換成基本磁碟。 |
| [轉換動態命令](convert-dynamic.md) | 將基本磁碟轉換為動態磁碟。 |
| [轉換 gpt 命令](convert-gpt.md) | 將具有主開機記錄 (MBR) 磁碟分割樣式的空基本磁碟轉換成具有 GUID 磁碟分割表格 (GPT) 磁碟分割樣式的基本磁碟。 |
| [轉換 mbr 命令](convert-mbr.md) | 將具有 GUID 磁碟分割表格 (GPT) 磁碟分割樣式的空白基本磁碟轉換為具有主開機記錄 (MBR) 磁碟分割樣式的基本磁碟。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

---
title: bitsadmin addfileset
description: 適用于**bitsadmin addfileset**的 Windows 命令主題，它會將一或多個檔案新增至指定的作業。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c4cff7dc8439fe8e1c54d1f5d231d1b487dc70c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850971"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

將一或多個檔案加入至指定的作業。

## <a name="syntax"></a>語法

```
bitsadmin /addfileset <Job> <TextFile>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| Job | 作業的顯示名稱或 GUID。 |
| TextFile | 文字檔，每一行包含遠端和本機檔案名。 **注意：** 名稱會以空格分隔。 以 `#` 字元開頭的行會被視為批註。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

```
C:\>bitsadmin /addfileset files.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
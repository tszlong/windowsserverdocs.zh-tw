---
title: 向
description: 公開命令的參考主題，會將持續性陰影複製公開為磁碟機號、共用或掛接點。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d4e8ebf71f6ddcb457460f8174793586e81c73a6
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437173"
---
# <a name="expose"></a>向

將持續性陰影複製公開為磁碟機號、共用或掛接點。

## <a name="syntax"></a>語法

```
expose <shadowID> {<drive:> | <share> | <mountpoint>}
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| shadowID | 指定您要公開之陰影複製的陰影識別碼。 您也可以使用現有的別名或環境變數來取代*shadowID*。 請使用不含參數的**add**來查看現有的別名。 |
| `<drive:>` | 將指定的陰影複製公開為磁碟機號（例如， `p:` ）。 |
| `<share>` | 在共用上公開指定的陰影複製（例如， `\\machinename` ）。   |
| `<mountpoint>` | 將指定的陰影複製公開到掛接點（例如， `C:\shadowcopy` ）。 |

### <a name="examples"></a>範例

若要將與 VSS_SHADOW_1 環境變數相關聯的持續陰影複製公開為磁片磁碟機 X，請輸入：

```
expose %vss_shadow_1% x:
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [diskshadow 命令](diskshadow.md)

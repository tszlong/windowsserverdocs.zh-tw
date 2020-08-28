---
title: expose
description: 公開命令的參考文章，此命令會將持續性陰影複製公開為磁碟機號、共用或掛接點。
ms.topic: reference
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 67e6b230b780e6ae84ea1ff30804c5722ca2337d
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036686"
---
# <a name="expose"></a>expose

將永久性陰影複製公開為磁碟機號、共用或掛接點。

## <a name="syntax"></a>語法

```
expose <shadowID> {<drive:> | <share> | <mountpoint>}
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| shadowID | 指定您想要公開之陰影複製的陰影識別碼。 您也可以使用現有的別名或環境變數來取代 *shadowID*。 使用不含參數的 **add** 來查看現有的別名。 |
| `<drive:>` | 公開指定的陰影複製做為磁碟機號 (例如 `p:`) 。 |
| `<share>` | 在共用 (公開指定的陰影複製，例如 `\\machinename`) 。   |
| `<mountpoint>` | 將指定的陰影複製公開至掛接點 (例如 `C:\shadowcopy`) 。 |

### <a name="examples"></a>範例

若要公開與 VSS_SHADOW_1 環境變數相關聯的持續性陰影複製做為磁片磁碟機 X，請輸入：

```
expose %vss_shadow_1% x:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [diskshadow 命令](diskshadow.md)

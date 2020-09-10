---
title: bitsadmin getmodificationtime
description: Bitsadmin getmodificationtime 命令的參考文章，此命令會抓取上次修改作業或成功傳輸資料的時間。
ms.topic: reference
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 293b8ef47374c0df3f9b1cd3d76802d592c3c522
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631933"
---
# <a name="bitsadmin-getmodificationtime"></a>bitsadmin getmodificationtime

捕獲上次修改作業或成功傳輸資料的時間。

## <a name="syntax"></a>語法

```
bitsadmin /getmodificationtime <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的上次修改時間：

```
bitsadmin /getmodificationtime myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

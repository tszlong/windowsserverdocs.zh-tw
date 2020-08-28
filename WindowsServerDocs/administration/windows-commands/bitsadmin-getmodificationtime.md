---
title: bitsadmin getmodificationtime
description: Bitsadmin getmodificationtime 命令的參考文章，此命令會抓取上次修改作業或成功傳輸資料的時間。
ms.topic: reference
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 763ca0f60fa834ad14b92eb7963107fa407a8964
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028726"
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

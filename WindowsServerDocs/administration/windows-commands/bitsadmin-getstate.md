---
title: bitsadmin getstate
description: Bitsadmin >getstate 命令的參考文章，此命令會抓取指定作業的狀態。
ms.topic: reference
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2faf05bd245f0b59eb9f10d0d46d96e8e1d3e11c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631652"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate

抓取指定作業的狀態。

## <a name="syntax"></a>語法

```
bitsadmin /getstate <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

#### <a name="output"></a>輸出

傳回的輸出值可以是：

| State | 描述 |
| --------------- | ----------- |
| 已排入佇列 | 作業正在等候執行。 |
| Connecting | BITS 正在聯繫伺服器。 |
| 傳輸中 | BITS 正在傳輸資料。 |
| 轉移 | BITS 已成功傳輸作業中的所有檔案。 |
| 暫止 | 作業已暫停。 |
| 錯誤 | 發生無法復原的錯誤;將不會重試傳送。 |
| Transient_Error | 發生可復原的錯誤;重試延遲的最小值過期時，會重試傳輸。 |
| 已認可 | 作業已完成。 |
| 已取消 | 已取消作業。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的狀態：

```
bitsadmin /getstate myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

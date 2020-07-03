---
title: bitsadmin getstate
description: Bitsadmin getstate 命令的參考文章，它會抓取指定之作業的狀態。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd698727cba25f15a12a331f847e7f8436d3d54e
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926674"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate

抓取指定之作業的狀態。

## <a name="syntax"></a>語法

```
bitsadmin /getstate <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

#### <a name="output"></a>輸出

傳回的輸出值可以是：

| State | Description |
| --------------- | ----------- |
| 已排入佇列 | 作業正在等候執行。 |
| Connecting | BITS 正在聯繫伺服器。 |
| 傳輸中 | BITS 正在傳輸資料。 |
| 交給 | BITS 已成功傳送作業中的所有檔案。 |
| 暫止 | 工作已暫停。 |
| 錯誤 | 發生無法復原的錯誤;將不會重試傳輸。 |
| Transient_Error | 發生可復原的錯誤;當最小重試延遲到期時，傳輸就會重試。 |
| 已認可 | 工作已完成。 |
| 已取消 | 已取消作業。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的狀態：

```
bitsadmin /getstate myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

---
title: bitsadmin getstate
description: Bitsadmin getstate 命令的參考文章，它會抓取指定之作業的狀態。
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38ad3cedbd4dc9b0cc3d5e855ea4fabd1736b6aa
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893846"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate

抓取指定之作業的狀態。

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

| 州 | 描述 |
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

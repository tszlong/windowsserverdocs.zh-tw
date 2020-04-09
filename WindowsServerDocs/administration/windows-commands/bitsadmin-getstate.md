---
title: bitsadmin getstate
description: 適用于**bitsadmin getstate**的 Windows 命令主題，它會抓取所指定工作的狀態。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43cd8c8e614cce65f55b16fc5395b1d37de0cf95
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850461"
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
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="output"></a>輸出

輸出值包括：

| 狀態 | 描述 |
| --------------- | ----------- |
| 已排入佇列 | 作業正在等候執行。 |
| 連線中 | BITS 正在聯繫伺服器。 |
| 地 | BITS 正在傳輸資料。 |
| 交給 | BITS 已成功傳送作業中的所有檔案。 |
| Suspended | 工作已暫停。 |
| 錯誤 | 發生無法復原的錯誤;將不會重試傳輸。 |
| Transient_Error | 發生可復原的錯誤;當最小重試延遲到期時，傳輸就會重試。 |
| 確認 | 工作已完成。 |
| Canceled | 已取消作業。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會抓取名為*myDownloadJob*之作業的狀態。

```
C:\>bitsadmin /getstate myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

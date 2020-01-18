---
title: bitsadmin getstate
description: 適用于 bitsadmin getstate 的 Windows 命令主題
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2cff790c8787b1514e8523a4583184d6f6a59efc
ms.sourcegitcommit: 51e0b575ef43cd16b2dab2db31c1d416e66eebe8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/17/2020
ms.locfileid: "76259103"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate


抓取指定之作業的狀態。

## <a name="syntax"></a>語法

```
bitsadmin /GetState <Job>
```

## <a name="parameters"></a>Parameters

| 參數 | 說明 |
| --------- | ----------- |
|    Job    | 作業的顯示名稱或 GUID |

## <a name="remarks"></a>備註

可能的狀態為：

|      州/省      | 說明 |
| --------------- | ----------- |
| 佇列          | 作業正在等候執行。 |
| 連接      | BITS 正在聯繫伺服器。 |
| 地    | BITS 正在傳輸資料。 |
| 交給     | BITS 已成功傳送作業中的所有檔案。 |
| 暫止       | 工作已暫停。 |
| 錯誤           | 發生無法復原的錯誤;將不會重試傳輸。 |
| TRANSIENT_ERROR | 發生可復原的錯誤;當最小重試延遲到期時，傳輸就會重試。 |
| 已認可    | 作業已完成。 |
| #A1        | 已取消作業。 |

## <a name="BKMK_examples"></a>範例

下列範例會抓取名為*myDownloadJob*之作業的狀態。

```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

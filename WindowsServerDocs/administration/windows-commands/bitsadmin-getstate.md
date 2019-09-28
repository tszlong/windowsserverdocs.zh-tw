---
title: bitsadmin getstate
description: '\* * * * 的 Windows 命令主題 '
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
ms.openlocfilehash: 55be37a6b1b44b81ed9002e5e3b9eb1fd46bd0dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381229"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate



抓取指定之作業的狀態。

## <a name="syntax"></a>語法

```
bitsadmin /GetState <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

可能的狀態為：

|-----|-----| |已排入佇列 |作業正在等候執行。 ||連接 |BITS 正在聯繫伺服器。 ||正在傳輸 |BITS 正在傳輸資料。 ||已暫停 |工作已暫停。 ||錯誤 |發生無法復原的錯誤;將不會重試傳送。 ||TRANSIENT_ERROR |發生可復原的錯誤;當最小重試延遲到期時，傳輸會重試。 ||已認可 |作業已完成。 ||已取消 |作業已取消。 |

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*之作業的狀態。
```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
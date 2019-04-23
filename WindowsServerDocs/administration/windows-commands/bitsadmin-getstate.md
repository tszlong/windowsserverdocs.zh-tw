---
title: bitsadmin getstate
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f7ed7529fda264efaceb6b4b36e36e728c211f3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889619"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate



擷取指定工作的狀態。

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

|-----|-----| |已排入佇列 |工作正在等候執行。 ||連接 |位元連絡伺服器。 ||傳輸 |位元傳輸資料。 ||暫止 |工作已暫停。 ||錯誤 |發生非可修復的錯誤不會重試傳輸。 ||TRANSIENT_ERROR |發生可復原的錯誤;傳輸重試最小的重試延遲過期時。 ||認可 |作業已完成。 ||取消 |工作已取消。 |

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業的狀態*myDownloadJob*。
```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
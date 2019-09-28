---
title: bitsadmin setmaxdownloadtime
description: '**Bitsadmin setmaxdownloadtime**的 Windows 命令主題-設定下載超時（以秒為單位）。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 985453de5bd2f4a06b5635ae5b0a9794d30175b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380565"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>bitsadmin setmaxdownloadtime



設定下載超時（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /SetMaxDownloadTime <Job> <Timeout>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|逾時|以秒為單位的超時時間|

## <a name="remarks"></a>備註

-   N/A

## <a name="BKMK_examples"></a>典型

下列範例會將名為*myDownloadJob*之作業的超時時間設定為10秒。
```
C:\>bitsadmin /SetMaxDownloadTime myDownloadJob 10
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
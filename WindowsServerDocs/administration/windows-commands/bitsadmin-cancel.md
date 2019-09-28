---
title: bitsadmin cancel
description: '**Bitsadmin 取消**的 Windows 命令主題-從傳輸佇列移除作業，並刪除與作業相關聯的所有暫存檔案。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 77e46d787359af43a37faba5d844bfec09730454
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381801"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

從傳輸佇列中移除作業，並刪除與作業相關聯的所有暫存檔案。

## <a name="syntax"></a>語法

```
bitsadmin /cancel <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型

下列範例會從傳送佇列中移除*myDownloadJob*作業。
```
C:\>bitsadmin /cancel myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
---
title: bitsadmin cache 和 setlimit
description: '**Bitsadmin cache 和 setlimit**的 Windows 命令主題-設定快取大小限制。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88a10ce8599202e237daa6822cf62806d3c21429
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381945"
---
# <a name="bitsadmin-cache-and-setlimit"></a>bitsadmin cache 和 setlimit



設定快取大小限制。

## <a name="syntax"></a>語法

```
bitsadmin /Cache /SetLimit Percent
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Percent|快取限制定義為總硬碟空間的百分比。|

## <a name="BKMK_examples"></a>典型

下列範例會將快取大小限制為 50%。
```
C:\>bitsadmin /Cache /SetLimit 50 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
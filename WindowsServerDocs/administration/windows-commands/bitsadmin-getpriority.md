---
title: bitsadmin getpriority
description: '**Bitsadmin getpriority**的 Windows 命令主題-抓取指定之作業的優先順序。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 0b8914f27c690aa9bb9cbf30430b3edf55f2eb92
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381427"
---
# <a name="bitsadmin-getpriority"></a>bitsadmin getpriority

抓取指定之作業的優先順序。

## <a name="syntax"></a>語法

```
bitsadmin /GetPriority <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

優先順序為 [**前景**]、[**高**]、[**一般**]、[**低**] 或 [**未知**]。

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*之作業的優先順序。
```
C:\>bitsadmin /GetPriority myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

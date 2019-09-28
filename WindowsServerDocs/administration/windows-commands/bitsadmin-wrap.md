---
title: bitsadmin wrap
description: 適用于**bitsadmin**的 Windows 命令主題會將任何會延伸到命令視窗最右邊邊緣的輸出文字換行至下一行。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5609fb6f38716795a545e0c7fe3939f893a8c8d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380686"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

包裝輸出以放入命令視窗中。

## <a name="syntax"></a>語法

```
bitsadmin /Wrap Job
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

在其他參數之前指定。 根據預設，除了[bitsadmin 監視器](bitsadmin-monitor.md)參數之外，所有參數都會包裝輸出。

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*之作業的資訊，並包裝輸出。

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

#### <a name="additional-references"></a>其他參考

[命令列語法關鍵](command-line-syntax-key.md)

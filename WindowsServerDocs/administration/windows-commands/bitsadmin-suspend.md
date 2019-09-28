---
title: bitsadmin suspend
description: '**Bitsadmin 暫停**的 Windows 命令主題-暫停指定的工作。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a3a484df2b50cdc8893512020b835f913793d2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380377"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> 適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

暫停指定的工作。

## <a name="syntax"></a>語法

```
bitsadmin /Suspend <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

若要重新開機作業，請使用[bitsadmin resume](bitsadmin-resume.md)參數。

## <a name="BKMK_examples"></a>典型

下列範例會暫停名為*myDownloadJob*的作業。

```
C:\>bitsadmin /Suspend myDownloadJob
```

#### <a name="additional-references"></a>其他參考

[命令列語法關鍵](command-line-syntax-key.md)

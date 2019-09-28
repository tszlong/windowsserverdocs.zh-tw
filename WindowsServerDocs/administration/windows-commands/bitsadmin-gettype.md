---
title: bitsadmin gettype
description: '**Bitsadmin gettype**的 Windows 命令主題-抓取指定作業的工作類型。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca46cb813809621f4fa79b3265198206729a392c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381345"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype



抓取指定之作業的工作類型。

## <a name="syntax"></a>語法

```
bitsadmin /GetType <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

類型可以是 [下載]、[上傳]、[上傳-回復] 或 [未知]。

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*之作業的工作類型。
```
C:\>bitsadmin /GetType myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
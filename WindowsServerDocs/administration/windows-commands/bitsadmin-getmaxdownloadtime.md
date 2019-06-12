---
title: bitsadmin getmaxdownloadtime
description: 適用於 Windows 命令主題**bitsadmin getmaxdownloadtime** -擷取下載逾時 （秒）。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cdce64f6-7125-489d-be3c-4af1dfc8c46a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d067d6a0821d9af4784c02c6a332e8eddd2352c0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434941"
---
# <a name="bitsadmin-getmaxdownloadtime"></a>bitsadmin getmaxdownloadtime

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

擷取下載逾時 （秒）。

## <a name="syntax"></a>語法

```
bitsadmin /GetMaxDownloadtime <Job> 
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

-   N\/A

## <a name="BKMK_examples"></a>範例
下列範例會取得名為作業的最大的下載時間*myDownloadJob*以秒為單位。

```
C:\>bitsadmin /GetMaxDownloadtime myDownloadJob
```

## <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)



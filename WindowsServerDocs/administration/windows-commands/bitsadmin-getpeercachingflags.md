---
title: bitsadmin getpeercachingflags
description: '**Bitsadmin getpeercachingflags**的 Windows 命令主題-取得旗標，以判斷作業的檔案是否可以快取並提供給對等，以及 BITS 是否可以從對等電腦下載作業的內容。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b86214b5289a59e8db2ecff065ab3b8cd17007e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381443"
---
# <a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取旗標，以判斷作業的檔案是否可以快取並提供給對等，以及 BITS 是否可以從對等電腦下載作業的內容。

## <a name="syntax"></a>語法

```
bitsadmin /GetPeerCachingFlags <Job> 
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型
下列範例會抓取名為*myJob*之作業的旗標。

```
C:\>bitsadmin /GetPeerCachingFlags myJob
```

## <a name="additional-references"></a>其他參考
[命令列語法關鍵](command-line-syntax-key.md)



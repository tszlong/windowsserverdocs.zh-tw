---
title: bitsadmin getpeercachingflags
description: 適用於 Windows 命令主題**bitsadmin getpeercachingflags** -擷取決定是否可以快取工作的檔案，並提供給對等，如果位元可以從對等的作業內容的旗標。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 28f248bab3e3cc3f5c7dd4f5f878f0b6d776029b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434922"
---
# <a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

擷取決定是否可以快取工作的檔案，並提供給對等，如果位元可以從對等的作業內容的旗標。

## <a name="syntax"></a>語法

```
bitsadmin /GetPeerCachingFlags <Job> 
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例
下列範例會擷取名為作業的旗標*myJob*。

```
C:\>bitsadmin /GetPeerCachingFlags myJob
```

## <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)



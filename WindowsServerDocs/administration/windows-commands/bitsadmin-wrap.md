---
title: bitsadmin wrap
description: 適用於 Windows 命令主題**bitsadmin 換行**-包裝輸出文字擴充至下一行的 [命令] 視窗的最右側邊緣以外的任何行。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4834a8a17c72394b6ee8f051ec76919af9880124
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881669"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

包裝輸出至命令視窗大小。

## <a name="syntax"></a>語法

```
bitsadmin /Wrap Job
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

指定其他參數之前。 根據預設，所有參數，除了[bitsadmin 監視器](bitsadmin-monitor.md)參數，輸出包裝。

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業的資訊*myDownloadJob*和包裝的輸出。

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

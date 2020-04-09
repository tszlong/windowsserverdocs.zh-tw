---
title: bitsadmin wrap
description: 適用于 bitsadmin wrap 的 Windows 命令主題，它會將任何行的輸出文字包裝到命令視窗的最右邊邊緣以外的下一行。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 009a0452f44c4944ae110ca6b9e0570793c32a72
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848751"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將延伸到命令視窗最右邊邊緣以外的任何一行輸出文字包裝到下一行。

## <a name="syntax"></a>語法

```
bitsadmin /Wrap Job
```

### <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

在其他參數之前指定。 根據預設，除了[bitsadmin 監視器](bitsadmin-monitor.md)參數之外，所有參數都會包裝輸出。

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會抓取名為*myDownloadJob*之作業的資訊，並包裝輸出。

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

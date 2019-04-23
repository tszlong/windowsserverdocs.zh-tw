---
title: bitsadmin getpriority
description: 適用於 Windows 命令主題**bitsadmin getpriority** -擷取指定工作的優先順序。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 6be2461ed87b75144367b1bd74376381e4674b66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841439"
---
# <a name="bitsadmin-getpriority"></a>bitsadmin getpriority

擷取指定工作的優先順序。

## <a name="syntax"></a>語法

```
bitsadmin /GetPriority <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

優先順序是**前景**，**高**，**一般**，**低**，或**未知**。

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業的優先權*myDownloadJob*。
```
C:\>bitsadmin /GetPriority myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

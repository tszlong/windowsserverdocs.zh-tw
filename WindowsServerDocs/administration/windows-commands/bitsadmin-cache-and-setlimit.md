---
title: bitsadmin 快取和 setlimit
description: 適用於 Windows 命令主題**bitsadmin 快取和 setlimit** -設定快取大小上限。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d0b72c5ec6c779fa4ce3fa038352836cd9456ac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852589"
---
# <a name="bitsadmin-cache-and-setlimit"></a>bitsadmin 快取和 setlimit



設定快取大小上限。

## <a name="syntax"></a>語法

```
bitsadmin /Cache /SetLimit Percent
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|百分比|定義的硬碟空間總計百分比的快取限制...|

## <a name="BKMK_examples"></a>範例

下列範例會限制為 50%的快取大小。
```
C:\>bitsadmin /Cache /SetLimit 50 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
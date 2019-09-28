---
title: convert
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae151297-af21-4701-bd69-21d775518e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d78f7adbc26acf9787ad39019e1450542a6acda2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379034"
---
# <a name="convert"></a>convert



將磁片從一種磁片類型轉換成另一種。

## <a name="syntax"></a>語法

```
convert basic
convert dynamic
convert gpt
convert mbr
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[轉換基本](convert-basic.md)|將空的動態磁碟轉換成基本磁碟。|
|[轉換動態](convert-dynamic.md)|將基本磁碟轉換成動態磁碟。|
|[轉換 gpt](convert-gpt.md)|將具有主開機記錄（MBR）磁碟分割樣式的空白基本磁碟轉換成具有 GUID 磁碟分割表格（GPT）磁碟分割樣式的基本磁碟。|
|[轉換 mbr](convert-mbr.md)|將具有 GUID 磁碟分割表格（GPT）磁碟分割樣式的空白基本磁碟，轉換為具有主開機記錄（MBR）磁碟分割樣式的基本磁碟。|

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


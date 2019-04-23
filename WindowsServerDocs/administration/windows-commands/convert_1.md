---
title: convert
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6a77e1fca9605c7e5cc4ff059db08ffbfcc81f81
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859279"
---
# <a name="convert"></a>convert



將磁碟從一個磁碟類型轉換到另一個。

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
|[基本轉換](convert-basic.md)|將空的動態磁碟轉換成基本磁碟。|
|[轉換動態](convert-dynamic.md)|將基本磁碟轉換為動態磁碟。|
|[轉換 gpt](convert-gpt.md)|將具有主開機記錄 (MBR) 磁碟分割樣式的空白基本磁碟轉換成具有 GUID 磁碟分割表格 (GPT) 磁碟分割樣式的基本磁碟。|
|[轉換 mbr](convert-mbr.md)|將具有 GUID 磁碟分割表格 (GPT) 磁碟分割樣式的空白基本磁碟轉換成具有主開機記錄 (MBR) 磁碟分割樣式的基本磁碟。|

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


---
title: convert
description: 適用于 convert 的 Windows 命令主題，可將磁片從一種磁片類型轉換成另一種。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ae151297-af21-4701-bd69-21d775518e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b1b5c62b7e51b497faac77a13185f844de230d67
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847181"
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

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[轉換基本](convert-basic.md)|將空的動態磁碟轉換為基本磁碟。|
|[轉換動態](convert-dynamic.md)|將基本磁碟轉換為動態磁碟。|
|[轉換 gpt](convert-gpt.md)|將具有主開機記錄 (MBR) 磁碟分割樣式的空基本磁碟轉換成具有 GUID 磁碟分割表格 (GPT) 磁碟分割樣式的基本磁碟。|
|[轉換 mbr](convert-mbr.md)|將具有 GUID 磁碟分割表格（GPT）磁碟分割樣式的空白基本磁碟，轉換為具有主開機記錄（MBR）磁碟分割樣式的基本磁碟。|

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)


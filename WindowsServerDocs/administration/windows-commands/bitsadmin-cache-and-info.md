---
title: bitsadmin 快取和資訊
description: 適用於 Windows 命令主題**bitsadmin 快取和資訊**-傾印的特定快取項目。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 61ff57b33e575921f2032d4e13a2d9b74accae60
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813319"
---
# <a name="bitsadmin-cache-and-info"></a>bitsadmin 快取和資訊



傾印的特定快取項目。

## <a name="syntax"></a>語法

```
bitsadmin /Cache /Info RecordID [/Verbose] 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|RecordID|使用快取項目相關聯的 GUID。|

## <a name="BKMK_examples"></a>範例

下列範例將傾印的 {6511FB02-E195-40A2-B595-E8E2F8F47702} 記錄識別碼的快取項目。
```
C:\>bitsadmin /Cache /Info {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
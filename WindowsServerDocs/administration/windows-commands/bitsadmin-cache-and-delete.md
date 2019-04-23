---
title: bitsadmin 快取和 delete
description: 適用於 Windows 命令主題**bitsadmin 快取並刪除**-刪除特定快取項目。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63b82cbbadebf2c4e36f2c76076b329787d7b1b5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852509"
---
# <a name="bitsadmin-cache-and-delete"></a>bitsadmin 快取和 delete



刪除特定快取項目。

## <a name="syntax"></a>語法

```
bitsadmin /Cache /Delete RecordID 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|RecordID|使用快取項目相關聯的 GUID。|

## <a name="BKMK_examples"></a>範例

下列範例會刪除的 {6511FB02-E195-40A2-B595-E8E2F8F47702} 記錄識別碼的快取項目。
```
C:\>bitsadmin /Cache /Delete {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
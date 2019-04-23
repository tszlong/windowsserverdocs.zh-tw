---
title: bitsadmin getvalidationstate
description: '適用於 Windows 命令主題**bitsadmin getvalidationstate** -報告指定的檔案，作業內的內容驗證狀態。 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8abff3fc9fddb9cff1758739fdc540a9c945efe2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879159"
---
# <a name="bitsadmin-getvalidationstate"></a>bitsadmin getvalidationstate



報告指定的檔案，作業內的內容驗證狀態。

## <a name="syntax"></a>語法

```
bitsadmin /GetValidationState <Job> <file index> 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|檔案索引|會從 0 開始|

## <a name="BKMK_examples"></a>範例

下列範例會取得名為作業內的檔案 2 的內容驗證狀態*myJob*。
```
C:\>bitsadmin /GetValidationState myJob 1
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
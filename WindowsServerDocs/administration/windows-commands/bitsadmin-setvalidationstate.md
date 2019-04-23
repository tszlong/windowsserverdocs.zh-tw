---
title: bitsadmin setvalidationstate
description: 適用於 Windows 命令主題**bitsadmin setvalidationstate** -設定指定的檔案，作業內的內容驗證狀態。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 647389aaac06d1eb109052548c1b24f7579bde2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851239"
---
# <a name="bitsadmin-setvalidationstate"></a>bitsadmin setvalidationstate



設定指定的檔案，作業內的內容驗證狀態。

## <a name="syntax"></a>語法

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|檔案索引|會從 0 開始|
|True|False|設定為 TRUE 的檔案內容是否有效，否則為 FALSE|

## <a name="BKMK_examples"></a>範例

下列範例會將檔案 2 的內容驗證狀態設定為適用於名為作業*myJob*。
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
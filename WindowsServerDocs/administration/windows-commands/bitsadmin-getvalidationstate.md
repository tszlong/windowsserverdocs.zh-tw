---
title: bitsadmin getvalidationstate
description: '**Bitsadmin getvalidationstate**的 Windows 命令主題-報告作業中指定檔案的內容驗證狀態。 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ca4269a596010258edd0479f5a7e9844bc9c98df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381265"
---
# <a name="bitsadmin-getvalidationstate"></a>bitsadmin getvalidationstate



報告作業中指定檔案的內容驗證狀態。

## <a name="syntax"></a>語法

```
bitsadmin /GetValidationState <Job> <file index> 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|檔案索引|從0開始|

## <a name="BKMK_examples"></a>典型

下列範例會在名為*myJob*的作業中，取得檔案2的內容驗證狀態。
```
C:\>bitsadmin /GetValidationState myJob 1
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
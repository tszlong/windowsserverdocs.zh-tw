---
title: bitsadmin setvalidationstate
description: '**Bitsadmin setvalidationstate**的 Windows 命令主題-設定作業中指定檔案的內容驗證狀態。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 37d7fa3a8a91abf1e7b6ac5a51b6cebd78984a91
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380405"
---
# <a name="bitsadmin-setvalidationstate"></a>bitsadmin setvalidationstate



設定作業中指定檔案的內容驗證狀態。

## <a name="syntax"></a>語法

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

## <a name="parameters"></a>參數

| 參數  |          描述           |
|------------|--------------------------------|
|    Job     | 作業的顯示名稱或 GUID |
| 檔案索引 |         從0開始          |
|    True    |             偽              |

## <a name="BKMK_examples"></a>典型

下列範例會針對名為*myJob*的作業，將檔案2的內容驗證狀態設定為 TRUE。
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
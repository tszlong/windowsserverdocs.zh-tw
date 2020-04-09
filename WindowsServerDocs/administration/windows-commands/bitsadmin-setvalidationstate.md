---
title: bitsadmin setvalidationstate
description: 適用于 bitsadmin setvalidationstate 的 Windows 命令主題，可設定作業中指定檔案的內容驗證狀態。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de6480596b55b3a483076297f32ce52a975915db
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849111"
---
# <a name="bitsadmin-setvalidationstate"></a>bitsadmin setvalidationstate

設定作業中指定檔案的內容驗證狀態。

## <a name="syntax"></a>語法

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

### <a name="parameters"></a>參數

| 參數  |          描述           |
|------------|--------------------------------|
|    Job     | 作業的顯示名稱或 GUID |
| 檔案索引 |         從0開始          |
|    True    |             False              |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會針對名為*myJob*的作業，將檔案2的內容驗證狀態設定為 TRUE。
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
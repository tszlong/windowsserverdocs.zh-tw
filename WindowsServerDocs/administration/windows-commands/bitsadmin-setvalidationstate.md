---
title: bitsadmin setvalidationstate
description: Bitsadmin setvalidationstate 命令的參考文章，此命令會在工作中設定指定檔案的內容驗證狀態。
ms.topic: reference
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5885f0f43e7c33e55dc05182819a339d69519d84
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034726"
---
# <a name="bitsadmin-setvalidationstate"></a>bitsadmin setvalidationstate

設定作業中指定檔案的內容驗證狀態。

## <a name="syntax"></a>語法

```
bitsadmin /setvalidationstate <job> <file_index> <TRUE|FALSE>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ---------- |
| 工作 (Job) | 作業的顯示名稱或 GUID。 |
| file_index | 從0開始。 |
| TRUE 或 FALSE | **TRUE** 會開啟指定檔案的內容驗證，而 **FALSE** 則會關閉它。 |

## <a name="examples"></a>範例

若要針對名為 *myDownloadJob*的作業，將檔案2的內容驗證狀態設為 TRUE：

```
bitsadmin /setvalidationstate myDownloadJob 2 TRUE
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

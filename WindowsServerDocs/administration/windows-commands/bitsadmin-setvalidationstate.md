---
title: bitsadmin setvalidationstate
description: Bitsadmin setvalidationstate 命令的參考文章，此命令會在工作中設定指定檔案的內容驗證狀態。
ms.topic: reference
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1f30807e392ede7710c4d7740416d2cdb34c1378
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630613"
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

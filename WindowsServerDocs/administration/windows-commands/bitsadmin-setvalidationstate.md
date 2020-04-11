---
title: bitsadmin setvalidationstate
description: 適用于**bitsadmin setvalidationstate**的 Windows 命令主題，可設定作業中指定檔案的內容驗證狀態。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6bec42ae926050cd21df594a38f1c441a40a527f
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122713"
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
| Job | 作業的顯示名稱或 GUID。 |
| file_index | 從0開始。 |
| TRUE 或 FALSE | **TRUE**會開啟指定檔案的內容驗證，而**FALSE**則關閉它。 |

## <a name="examples"></a>範例

下列範例會針對名為*myDownloadJob*的作業，將檔案2的內容驗證狀態設定為 TRUE。

```
C:\>bitsadmin /setvalidationstate myDownloadJob 2 TRUE
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
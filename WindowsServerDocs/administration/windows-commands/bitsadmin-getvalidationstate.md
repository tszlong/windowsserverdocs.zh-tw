---
title: bitsadmin getvalidationstate
description: 適用于**bitsadmin getvalidationstate**的 Windows 命令主題，它會報告作業中指定檔案的內容驗證狀態。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52d7d983cc7858607c350483ed81223d107cee25
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850431"
---
# <a name="bitsadmin-getvalidationstate"></a>bitsadmin getvalidationstate

報告作業中指定檔案的內容驗證狀態。

## <a name="syntax"></a>語法

```
bitsadmin /getvalidationstate <job> <file_index>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| file_index | 從0開始。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會在名為*myDownloadJob*的作業中，取得檔案2的內容驗證狀態。

```
C:\>bitsadmin /getvalidationstate myDownloadJob 1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
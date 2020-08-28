---
title: bitsadmin peercaching and getconfigurationflags
description: Bitsadmin 對等互連和 getconfigurationflags 命令的參考文章，可取得設定旗標，以判斷電腦是否將內容提供給對等，以及是否可以從對等下載內容。
ms.topic: reference
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab2a03a4b4dd7aa63abf0285808009a7b9cd04e6
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026566"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>bitsadmin peercaching and getconfigurationflags

取得設定旗標，以判斷電腦是否將內容提供給對等，以及是否可以從對等下載內容。

## <a name="syntax"></a>語法

```
bitsadmin /peercaching /getconfigurationflags <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的設定旗標：

```
bitsadmin /peercaching /getconfigurationflags myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

- [bitsadmin 對等的命令](bitsadmin-peercaching.md)

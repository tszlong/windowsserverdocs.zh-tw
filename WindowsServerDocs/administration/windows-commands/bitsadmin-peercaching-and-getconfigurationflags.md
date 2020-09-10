---
title: bitsadmin peercaching and getconfigurationflags
description: Bitsadmin 對等互連和 getconfigurationflags 命令的參考文章，可取得設定旗標，以判斷電腦是否將內容提供給對等，以及是否可以從對等下載內容。
ms.topic: reference
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3fe1adfb44ab845ecdb73222131191782f83c075
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631377"
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

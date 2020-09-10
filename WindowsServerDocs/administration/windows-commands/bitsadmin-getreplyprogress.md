---
title: bitsadmin getreplyprogress
description: Bitsadmin getreplyprogress 命令的參考文章，可抓取伺服器上傳-回復的大小和進度。
ms.topic: reference
ms.assetid: 7f7cb0b4-ad95-44fd-a35d-0ddf5fc0b0d0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7dffacb53044175b8c65d3832863d17d59853b3a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631694"
---
# <a name="bitsadmin-getreplyprogress"></a>bitsadmin getreplyprogress

抓取伺服器上傳-回復的大小和進度。

> [!NOTE]
> BITS 1.2 及更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /getreplyprogress <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的上傳回復進度：

```
bitsadmin /getreplyprogress myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

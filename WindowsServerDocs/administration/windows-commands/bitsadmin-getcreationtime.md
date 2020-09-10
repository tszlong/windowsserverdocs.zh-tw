---
title: bitsadmin getcreationtime
description: Bitsadmin getcreationtime 命令的參考文章，此命令會抓取指定作業的建立時間。
ms.topic: reference
ms.assetid: be409cb5-ce72-41d9-aafa-edd4e230fd14
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 780f7124248bd38f0c3d7cc9e7cb14b1a9decc03
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632234"
---
# <a name="bitsadmin-getcreationtime"></a>bitsadmin getcreationtime

抓取指定作業的建立時間。

## <a name="syntax"></a>語法

```
bitsadmin /getcreationtime <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的建立時間：

```
bitsadmin /getcreationtime myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

---
title: bitsadmin getfilestransferred
description: Bitsadmin getfilestransferred 命令的參考文章，此命令會抓取針對指定工作傳輸的檔數目。
ms.topic: reference
ms.assetid: e282815c-938b-4ac0-a09d-9baafb656dcb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b4a34e799b2d7e41373a1b4682c77e3ad93d36c2
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632092"
---
# <a name="bitsadmin-getfilestransferred"></a>bitsadmin getfilestransferred

抓取針對指定工作傳輸的檔案數目。

## <a name="syntax"></a>語法

```
bitsadmin /getfilestransferred <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取出在名為 *myDownloadJob*的作業中傳輸的檔案數目：

```
bitsadmin /getfilestransferred myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

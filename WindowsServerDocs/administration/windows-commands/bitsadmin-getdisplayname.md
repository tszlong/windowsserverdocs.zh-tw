---
title: bitsadmin getdisplayname
description: Bitsadmin getdisplayname 命令的參考文章，此命令會抓取指定作業的顯示名稱。
ms.topic: reference
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2e106f9b1815d735502ee451ec59c7f34f9ea063
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632129"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname

抓取指定作業的顯示名稱。

## <a name="syntax"></a>語法

```
bitsadmin /getdisplayname <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的顯示名稱：

```
bitsadmin /getdisplayname myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

---
title: bitsadmin getcompletiontime
description: Bitsadmin getcompletiontime 命令的參考文章，此命令會抓取作業完成傳送資料的時間。
ms.topic: reference
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 660a28dcfe99bd5801ba2fc3ad909cec78a671ce
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632242"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime

抓取作業完成傳送資料的時間。

## <a name="syntax"></a>語法

```
bitsadmin /getcompletiontime <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob* 的作業完成傳送資料的時間：

```
bitsadmin /getcompletiontime myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

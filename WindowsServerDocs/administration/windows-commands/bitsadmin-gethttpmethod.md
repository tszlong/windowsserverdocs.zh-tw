---
title: bitsadmin gethttpmethod
description: Bitsadmin getHTTPmethod 命令的參考文章，這個命令會取得與作業搭配使用的 HTTP 動詞命令。
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 03/01/2019
ms.openlocfilehash: 0d96c85aa99330b133954a77ca5584fe2d02cd43
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632029"
---
# <a name="bitsadmin-gethttpmethod"></a>bitsadmin gethttpmethod

取得要與作業搭配使用的 HTTP 動詞命令。

## <a name="syntax"></a>語法

```
bitsadmin /gethttpmethod <Job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得 HTTP 動詞命令，以與名為 *myDownloadJob*的作業搭配使用：

```
bitsadmin /gethttpmethod myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

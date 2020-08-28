---
title: bitsadmin gethttpmethod
description: Bitsadmin getHTTPmethod 命令的參考文章，這個命令會取得與作業搭配使用的 HTTP 動詞命令。
ms.topic: reference
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 5bbd2509cabea7ae68240a31cee78d9ffd3ac5e0
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033526"
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

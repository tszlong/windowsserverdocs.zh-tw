---
title: bitsadmin gethttpmethod
description: Bitsadmin getHTTPmethod 命令的參考文章，可取得要搭配作業使用的 HTTP 指令動詞。
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: ea6f1b3896b11bfcc157ea54dc0470786d3dff1f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894221"
---
# <a name="bitsadmin-gethttpmethod"></a>bitsadmin gethttpmethod

取得要搭配作業使用的 HTTP 指令動詞。

## <a name="syntax"></a>語法

```
bitsadmin /gethttpmethod <Job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取出 HTTP 動詞命令以與名為*myDownloadJob*的作業搭配使用：

```
bitsadmin /gethttpmethod myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

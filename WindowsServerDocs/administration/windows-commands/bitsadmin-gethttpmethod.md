---
title: bitsadmin gethttpmethod
description: Bitsadmin getHTTPmethod 命令的參考文章，可取得要搭配作業使用的 HTTP 指令動詞。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 3a963eb275c6e635849094906c52fedf90dccd0d
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927029"
---
# <a name="bitsadmin-gethttpmethod"></a>bitsadmin gethttpmethod

取得要搭配作業使用的 HTTP 指令動詞。

## <a name="syntax"></a>語法

```
bitsadmin /gethttpmethod <Job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
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

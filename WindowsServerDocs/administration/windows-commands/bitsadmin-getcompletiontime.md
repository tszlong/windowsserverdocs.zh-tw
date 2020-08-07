---
title: bitsadmin getcompletiontime
description: Bitsadmin getcompletiontime 命令的參考文章，它會抓取作業完成傳輸資料的時間。
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a242ec3689bef6e4ecd2bb961f532efbb874e98c
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894471"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime

抓取作業完成資料傳輸的時間。

## <a name="syntax"></a>語法

```
bitsadmin /getcompletiontime <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取出名為*myDownloadJob*的作業完成傳輸資料的時間：

```
bitsadmin /getcompletiontime myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

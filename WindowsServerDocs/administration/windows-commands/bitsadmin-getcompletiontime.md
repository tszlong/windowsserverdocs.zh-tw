---
title: bitsadmin getcompletiontime
description: Bitsadmin getcompletiontime 命令的參考文章，它會抓取作業完成傳輸資料的時間。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e07dd6a345cd1bd58277ef08e08802a62d6e6772
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923105"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime

抓取作業完成資料傳輸的時間。

## <a name="syntax"></a>語法

```
bitsadmin /getcompletiontime <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
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

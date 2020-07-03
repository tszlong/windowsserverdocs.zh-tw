---
title: bitsadmin getmaxdownloadtime
description: Bitsadmin getmaxdownloadtime 命令的參考文章，它會抓取下載超時（以秒為單位）。
ms.prod: windows-servemr
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cdce64f6-7125-489d-be3c-4af1dfc8c46a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9737e25e05b372cb6bb1057cc3a60fd5e2cf0ca8
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928177"
---
# <a name="bitsadmin-getmaxdownloadtime"></a>bitsadmin getmaxdownloadtime

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取下載超時（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /getmaxdownloadtime <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的下載時間上限（以秒為單位）：

```
bitsadmin /getmaxdownloadtime myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

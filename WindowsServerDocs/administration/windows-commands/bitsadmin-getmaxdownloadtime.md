---
title: bitsadmin getmaxdownloadtime
description: Bitsadmin getmaxdownloadtime 命令的參考文章，此命令會抓取下載超時（以秒為單位）。
ms.prod: windows-servemr
ms.topic: reference
ms.assetid: cdce64f6-7125-489d-be3c-4af1dfc8c46a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 610553a839bb36bd764da1283ba398af49824731
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030336"
---
# <a name="bitsadmin-getmaxdownloadtime"></a>bitsadmin getmaxdownloadtime

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

以秒為單位捕獲下載超時。

## <a name="syntax"></a>語法

```
bitsadmin /getmaxdownloadtime <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob* 之作業的最大下載時間（以秒為單位）：

```
bitsadmin /getmaxdownloadtime myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

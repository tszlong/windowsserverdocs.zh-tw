---
title: bitsadmin getmaxdownloadtime
description: 適用于**bitsadmin getmaxdownloadtime**的 Windows 命令主題，它會抓取下載超時（以秒為單位）。
ms.prod: windows-servemr
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cdce64f6-7125-489d-be3c-4af1dfc8c46a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b6e4a45da76d5ba39edae151454ad7f28a74085
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850631"
---
# <a name="bitsadmin-getmaxdownloadtime"></a>bitsadmin getmaxdownloadtime

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取下載超時（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /getmaxdownloadtime <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會取得名為*myDownloadJob*之作業的下載時間上限（以秒為單位）。

```
C:\>bitsadmin /getmaxdownloadtime myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

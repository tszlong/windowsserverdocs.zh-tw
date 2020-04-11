---
title: bitsadmin setmaxdownloadtime
description: 適用于**bitsadmin setmaxdownloadtime**的 Windows 命令主題，會設定下載超時（以秒為單位）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f07931dfb9fabaec272384dced6d60f1335b6a94
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122917"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>bitsadmin setmaxdownloadtime

設定下載超時（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /setmaxdownloadtime <job> <timeout>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| timeout | 下載超時時間長度（以秒為單位）。 |

## <a name="examples"></a>範例

下列範例會將名為*myDownloadJob*之作業的超時時間設定為10秒。

```
C:\>bitsadmin /setmaxdownloadtime myDownloadJob 10
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
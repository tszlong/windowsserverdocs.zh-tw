---
title: bitsadmin cancel
description: Bitsadmin cancel 命令的參考文章，它會從傳送佇列中移除作業，並刪除與作業相關聯的所有暫存檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 35173fe8b2e4f3888fa3a365ca25da35b8153eb4
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928368"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

從傳輸佇列中移除作業，並刪除與作業相關聯的所有暫存檔案。

## <a name="syntax"></a>語法

```
bitsadmin /cancel <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要從傳送佇列中移除*myDownloadJob*作業：

```
bitsadmin /cancel myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

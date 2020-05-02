---
title: bitsadmin cancel
description: Bitsadmin cancel 命令的參考主題，它會從傳送佇列中移除工作，並刪除與作業相關聯的所有暫存檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 95fefbc4a9731c2ccbac22adc27f8231a7f36138
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718258"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

從傳輸佇列中移除作業，並刪除與作業相關聯的所有暫存檔案。

## <a name="syntax"></a>語法

```
bitsadmin /cancel <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要從傳送佇列中移除*myDownloadJob*作業：

```
bitsadmin /cancel myDownloadJob
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

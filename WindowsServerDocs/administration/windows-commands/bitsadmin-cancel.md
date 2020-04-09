---
title: bitsadmin cancel
description: '**Bitsadmin cancel**的 Windows 命令主題，這會從傳送佇列中移除作業，並刪除與作業相關聯的所有暫存檔案。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c2bdeef824bc269671cc5ae926fb77cd5726c58
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850831"
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
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會從傳送佇列中移除*myDownloadJob*作業。

```
C:\>bitsadmin /cancel myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
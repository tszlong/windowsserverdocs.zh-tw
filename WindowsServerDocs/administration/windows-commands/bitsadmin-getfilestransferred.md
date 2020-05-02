---
title: bitsadmin getfilestransferred
description: Bitsadmin getfilestransferred 命令的參考主題，它會抓取所指定工作的已傳輸檔案數目。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e282815c-938b-4ac0-a09d-9baafb656dcb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed11739029338ecce5fc4efbe1918873a7f37f62
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717921"
---
# <a name="bitsadmin-getfilestransferred"></a>bitsadmin getfilestransferred

抓取指定作業所傳輸的檔案數目。

## <a name="syntax"></a>語法

```
bitsadmin /getfilestransferred <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業中傳輸的檔案數目：

```
bitsadmin /getfilestransferred myDownloadJob
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

---
title: bitsadmin getpriority
description: Bitsadmin getpriority 命令的參考主題，它會抓取指定之作業的優先順序。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 38f92e83ccf5b048d168ce6a21c6026f490b18bf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717681"
---
# <a name="bitsadmin-getpriority"></a>bitsadmin getpriority

抓取指定之作業的優先順序。

## <a name="syntax"></a>語法

```
bitsadmin /getpriority <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

#### <a name="output"></a>輸出

此命令傳回的優先順序可以是：

- **提到**

- **HIGH**

- **一般**

- **LOW**

- **未知**

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的優先順序：

```
bitsadmin /getpriority myDownloadJob
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

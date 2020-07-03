---
title: bitsadmin getpriority
description: Bitsadmin getpriority 命令的參考文章，它會抓取指定之作業的優先順序。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 39ce769e2eb1dcf7a05d416dc2a66c6c082c9ae4
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926856"
---
# <a name="bitsadmin-getpriority"></a>bitsadmin getpriority

抓取指定之作業的優先順序。

## <a name="syntax"></a>語法

```
bitsadmin /getpriority <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

#### <a name="output"></a>輸出

此命令傳回的優先順序可以是：

- **提到**

- **更**

- **一般**

- **量**

- **UNKNOWN**

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的優先順序：

```
bitsadmin /getpriority myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

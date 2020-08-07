---
title: bitsadmin getpriority
description: Bitsadmin getpriority 命令的參考文章，它會抓取指定之作業的優先順序。
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 57d51e4a2a34fb5ae1361e864ee932ac314662b2
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894039"
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

- **NORMAL**

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

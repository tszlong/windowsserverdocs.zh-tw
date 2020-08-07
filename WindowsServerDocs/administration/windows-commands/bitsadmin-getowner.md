---
title: bitsadmin getowner
description: Bitsadmin getowner 命令的參考文章，它會抓取指定之作業的擁有者。
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91dd63dcba8b41fee01c17e87c817e32a9dbba39
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894044"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

顯示指定作業之擁有者的顯示名稱或 GUID。

## <a name="syntax"></a>語法

```
bitsadmin /getowner <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要顯示名為*myDownloadJob*之作業的擁有者：

```
bitsadmin /getowner myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

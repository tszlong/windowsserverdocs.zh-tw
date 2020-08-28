---
title: bitsadmin takeownership
description: Bitsadmin takeownership 命令的參考文章，此命令可讓具有系統管理許可權的使用者取得指定工作的擁有權。
ms.topic: reference
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0df40e336ecc282e4b1a1774d7b5848f37a1a7a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033376"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership

讓具有系統管理許可權的使用者取得指定工作的擁有權。

## <a name="syntax"></a>語法

```
bitsadmin /takeownership <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ---------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

取得名為 *myDownloadJob*之作業的擁有權：

```
bitsadmin /takeownership myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

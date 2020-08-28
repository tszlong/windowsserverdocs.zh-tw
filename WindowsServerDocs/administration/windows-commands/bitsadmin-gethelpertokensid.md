---
title: bitsadmin gethelpertokensid
description: Bitsadmin gethelpertokensid 命令的參考文章，此命令會傳回 BITS 傳送工作的協助程式權杖 SID （如果有設定的話）。
ms.topic: reference
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 9cfa2c2a10d1f3947374262bbeb9ab21e559cfee
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033546"
---
# <a name="bitsadmin-gethelpertokensid"></a>bitsadmin gethelpertokensid

傳回 BITS 傳送工作的協助程式 [權杖](/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)SID （如果已設定）。

> [!NOTE]
> BITS 3.0 及更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /gethelpertokensid <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

取得名為 *myDownloadJob*之 BITS 傳送工作的 SID：

```
bitsadmin /gethelpertokensid myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

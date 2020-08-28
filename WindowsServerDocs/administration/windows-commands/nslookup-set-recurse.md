---
title: nslookup set recurse
description: Nslookup set 遞迴命令的參考文章，如果找不到指定伺服器上的資訊，則會告知網域名稱系統 (DNS) 名稱伺服器來查詢其他伺服器。
ms.topic: reference
ms.assetid: d1b7a93f-dfb0-4ccd-b230-e0953057fada
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 57c881dc101df2ebf8d659f29340a9fcb574cce6
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025202"
---
# <a name="nslookup-set-recurse"></a>nslookup set recurse

如果找不到指定伺服器上的資訊，則會告知網域名稱系統 (DNS) 名稱伺服器來查詢其他伺服器。

## <a name="syntax"></a>語法

```
set [no]recurse
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ---------- |
| norecurse | 如果找不到指定伺服器上的資訊，請停止網域名稱系統 (DNS) 名稱伺服器查詢其他伺服器。 |
| 遞迴 | 如果找不到指定伺服器上的資訊，則會告知網域名稱系統 (DNS) 名稱伺服器來查詢其他伺服器。 這是預設值。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

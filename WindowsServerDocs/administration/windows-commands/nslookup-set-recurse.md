---
title: nslookup set recurse
description: Nslookup set 遞迴命令的參考文章，如果找不到指定伺服器上的資訊，則會告知網域名稱系統 (DNS) 名稱伺服器來查詢其他伺服器。
ms.topic: reference
ms.assetid: d1b7a93f-dfb0-4ccd-b230-e0953057fada
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c33767e400e7bfcda0792af23673889f0afd7de9
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639415"
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

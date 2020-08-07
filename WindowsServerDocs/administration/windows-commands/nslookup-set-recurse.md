---
title: nslookup set recurse
description: Nslookup set 遞迴命令的參考文章，如果找不到指定伺服器上的資訊，則會告訴網域名稱系統 (DNS) 名稱伺服器來查詢其他伺服器。
ms.topic: article
ms.assetid: d1b7a93f-dfb0-4ccd-b230-e0953057fada
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26a98e646f0915a684129d4b0205384f10c31d49
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885570"
---
# <a name="nslookup-set-recurse"></a>nslookup set recurse

如果找不到指定伺服器上的資訊，則告訴網域名稱系統 (DNS) 名稱伺服器來查詢其他伺服器。

## <a name="syntax"></a>語法

```
set [no]recurse
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ---------- |
| norecurse | 如果 DNS) 名稱伺服器找不到指定伺服器上的資訊，則會停止查詢其他伺服器的網域名稱系統 (。 |
| 遞迴 | 如果找不到指定伺服器上的資訊，則告訴網域名稱系統 (DNS) 名稱伺服器來查詢其他伺服器。 這是預設值。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

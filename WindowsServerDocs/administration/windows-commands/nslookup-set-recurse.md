---
title: nslookup set recurse
description: Nslookup set 遞迴命令的參考文章，如果找不到指定伺服器上的資訊，會告訴網域名稱系統（DNS）名稱伺服器查詢其他伺服器。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d1b7a93f-dfb0-4ccd-b230-e0953057fada
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dd65eaa9ba60e2a3cdf79e808b2efccb28b7d455
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935692"
---
# <a name="nslookup-set-recurse"></a>nslookup set recurse

當網域名稱系統（DNS）名稱伺服器找不到指定之伺服器上的資訊時，會告訴他們查詢其他伺服器。

## <a name="syntax"></a>語法

```
set [no]recurse
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| ---------- | ---------- |
| norecurse | 如果網域名稱系統（DNS）名稱伺服器找不到指定伺服器上的資訊，則會停止查詢其他伺服器。 |
| 遞迴 | 當網域名稱系統（DNS）名稱伺服器找不到指定之伺服器上的資訊時，會告訴他們查詢其他伺服器。 這是預設值。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

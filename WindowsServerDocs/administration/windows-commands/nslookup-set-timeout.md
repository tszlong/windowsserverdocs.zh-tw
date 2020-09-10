---
title: nslookup set timeout
description: Nslookup set timeout 命令的參考文章，這會變更等候查閱要求回復的初始秒數。
ms.topic: reference
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b470598378f1b338da209ea16a0d8102177c573c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640526"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更等候查閱要求回復的初始秒數。 如果未在指定的一段時間內收到回復，則會將超時期限加倍，並重新傳送要求。 使用 [nslookup set retry](nslookup-set-retry.md) 命令來決定嘗試傳送要求的次數。

## <a name="syntax"></a>語法

```
set timeout=<number>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ---------- |
| `<number>` | 指定等候回復的秒數。 等候的預設秒數為 **5**。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要將取得回應的時間設定為2秒：

```
set timeout=2
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup set retry](nslookup-set-retry.md)

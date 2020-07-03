---
title: nslookup set timeout
description: Nslookup set timeout 命令的參考文章，會變更等候查閱要求回復的初始秒數。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6df8d1229dd57a84cb0dced3829bb328e41f092c
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930341"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更等候查閱要求回復的初始秒數。 如果未在指定的時間內收到回復，則超時期間會加倍，而且會重新傳送要求。 使用[nslookup 設定重試](nslookup-set-retry.md)命令，判斷嘗試傳送要求的次數。

## <a name="syntax"></a>語法

```
set timeout=<number>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| ---------- | ---------- |
| `<number>` | 指定等待回復的秒數。 預設等候的秒數為**5**。 |
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

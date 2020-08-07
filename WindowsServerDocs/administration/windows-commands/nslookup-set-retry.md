---
title: nslookup set retry
description: Nslookup 設定重試命令的參考文章，其會設定嘗試從指定伺服器取得資訊的次數。
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b4449be93c4587dcb5d1a7990a79352a1fa7b28
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885555"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果在一段時間內未收到回復，則超時時間會加倍，並會重新傳送要求。 此命令會設定要求重新傳送至伺服器以取得資訊的次數，然後再放棄。

> [!NOTE]
> 若要變更要求超時前的時間長度，請使用[nslookup set timeout](nslookup-set-timeout.md)命令。

## <a name="syntax"></a>語法

```
set retry=<number>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ---------- |
| `<number>` | 指定重試次數的新值。 預設的重試次數為**4**。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup set timeout](nslookup-set-timeout.md)

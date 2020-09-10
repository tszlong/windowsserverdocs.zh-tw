---
title: nslookup set retry
description: Nslookup set retry 命令的參考文章，設定嘗試從指定的伺服器取得資訊的次數。
ms.topic: reference
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: fa8b588c49b61c2269325b1b9e67d350f837c5ad
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637523"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果在特定一段時間內未收到回復，則會將超時期限加倍，並重新傳送要求。 此命令會設定在放棄之前，要求重新傳送到伺服器以取得資訊的次數。

> [!NOTE]
> 若要變更要求超時之前的時間長度，請使用 [nslookup set timeout](nslookup-set-timeout.md) 命令。

## <a name="syntax"></a>語法

```
set retry=<number>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ---------- |
| `<number>` | 指定重試次數的新值。 預設的重試次數為 **4**。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup set timeout](nslookup-set-timeout.md)

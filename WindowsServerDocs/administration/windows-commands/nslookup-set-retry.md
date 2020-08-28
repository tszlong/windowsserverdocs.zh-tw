---
title: nslookup set retry
description: Nslookup set retry 命令的參考文章，設定嘗試從指定的伺服器取得資訊的次數。
ms.topic: reference
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 10f60b48485ae51d727f17a5cfcec8b2af61a743
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025172"
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

---
title: nslookup set retry
description: Nslookup 設定重試命令的參考主題，其會設定嘗試從指定伺服器取得資訊的次數。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 268a9f0023c0e7e19e8ed413895f639444fe3b88
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721461"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup set timeout](nslookup-set-timeout.md)

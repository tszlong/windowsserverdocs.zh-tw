---
title: nslookup set retry
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1baeeaefedc211434f46bd0cfad713f093a873bf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723588"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定重試次數。
## <a name="syntax"></a>語法
```
set retry=<Number>
```
### <a name="parameters"></a>參數

|    參數    |                                      描述                                       |
|-----------------|----------------------------------------------------------------------------------------|
|    <Number>     | 指定重試次數的新值。 預設的重試次數為4。 |
| {help &#124;？} |                 顯示**nslookup**子命令的簡短摘要。                  |

## <a name="remarks"></a>備註
- 在特定時間內未收到要求的回復時，超時期間會加倍，而且會重新傳送要求。 重試值會控制要求在放棄之前重新傳送的次數。 您可以使用**set timeout**子命令來變更超時時間。
  ## <a name="additional-references"></a>其他參考
  - [命令列語法索引鍵](command-line-syntax-key.md)
  [nslookup 設定 timeout](nslookup-set-timeout.md)

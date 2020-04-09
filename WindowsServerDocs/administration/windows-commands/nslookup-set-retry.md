---
title: nslookup set retry
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2b95c4c8af2d7960270fd43f7a766b313ddbc07a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838341"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定重試次數。
## <a name="syntax"></a>語法
```
set retry=<Number>
```
### <a name="parameters"></a>參數

|    參數    |                                      描述                                       |
|-----------------|----------------------------------------------------------------------------------------|
|    <Number>     | 指定重試次數的新值。 預設的重試次數為4。 |
| {help &#124; ？} |                 顯示**nslookup**子命令的簡短摘要。                  |

## <a name="remarks"></a>備註
- 在特定時間內未收到要求的回復時，超時期間會加倍，而且會重新傳送要求。 重試值會控制要求在放棄之前重新傳送的次數。 您可以使用**set timeout**子命令來變更超時時間。
  ## <a name="additional-references"></a>其他參考資料
  - [命令列語法索引鍵](command-line-syntax-key.md)
  [nslookup 設定超時](nslookup-set-timeout.md)

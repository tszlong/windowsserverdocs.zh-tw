---
title: nslookup set retry
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 306bcc4f5e7ac98767c3c2e274100cf917874a8e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372865"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定重試次數。
## <a name="syntax"></a>語法
```
set retry=<Number>
```
## <a name="parameters"></a>Parameters

|    參數    |                                      描述                                       |
|-----------------|----------------------------------------------------------------------------------------|
|    <Number>     | 指定重試次數的新值。 預設的重試次數為4。 |
| {help &#124; ？} |                 顯示**nslookup**子命令的簡短摘要。                  |

## <a name="remarks"></a>備註
- 在特定時間內未收到要求的回復時，超時期間會加倍，而且會重新傳送要求。 重試值會控制要求在放棄之前重新傳送的次數。 您可以使用**set timeout**子命令來變更超時時間。
  ## <a name="additional-references"></a>其他參考
  [命令列語法索引鍵](command-line-syntax-key.md)
  [nslookup 設定超時](nslookup-set-timeout.md)

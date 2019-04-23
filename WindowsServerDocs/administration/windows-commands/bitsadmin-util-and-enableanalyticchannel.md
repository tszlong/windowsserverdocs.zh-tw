---
title: bitsadmin util 和 enableanalyticchannel
description: 適用於 Windows 命令主題**bitsadmin util 和 enableanalyticchannel** -啟用或停用位元用戶端分析通道。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0d7645aa-b91b-4ed7-b630-a1e1be6f6ae9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 814442a4d9b1a4d6e45b28f41a89b7a144be1cbf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877319"
---
# <a name="bitsadmin-util-and-enableanalyticchannel"></a>bitsadmin util 和 enableanalyticchannel



啟用或停用位元用戶端分析通道。

## <a name="syntax"></a>語法

```
bitsadmin /Util /EnableAnalyticChannel TRUE|FALSE
```

## <a name="BKMK_examples"></a>範例

下列範例會啟用 BITS 用戶端分析通道。
```
C:\>bitsadmin /Util / EnableAnalyticChannel TRUE
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
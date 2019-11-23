---
title: nslookup set root
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a1737275bf6321525bbba56cd4d6a77ef973423
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591021"
---
# <a name="nslookup-set-root"></a>nslookup set root

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更用於查詢的根功能變數名稱稱。
## <a name="syntax"></a>語法
```
set root=<RootServer>
```
## <a name="parameters"></a>Parameters

|    參數    |                                   描述                                    |
|-----------------|----------------------------------------------------------------------------------|
|  <RootServer>   | 指定根功能變數名稱伺服器的新名稱。 預設值為 ns.nic.ddn.mil。 |
| {help &#124; ？} |              顯示**nslookup**子命令的簡短摘要。               |

## <a name="remarks"></a>備註
- **Set root**子命令會影響**根**子命令。
  ## <a name="additional-references"></a>其他參考
  [命令列語法索引鍵](command-line-syntax-key.md)
  [nslookup 根目錄](nslookup-root.md)

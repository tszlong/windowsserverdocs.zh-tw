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
ms.openlocfilehash: 08cf41ec9b6ac30699013112216a538dcf625fd5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372843"
---
# <a name="nslookup-set-root"></a>nslookup set root

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更用於查詢的根功能變數名稱稱。
## <a name="syntax"></a>語法
```
set root=<RootServer>
```
## <a name="parameters"></a>參數

|    參數    |                                   描述                                    |
|-----------------|----------------------------------------------------------------------------------|
|  <RootServer>   | 指定根功能變數名稱伺服器的新名稱。 預設值為 ns.nic.ddn.mil。 |
| {help &#124; ？} |              顯示**nslookup**子命令的簡短摘要。               |

## <a name="remarks"></a>備註
- **Set root**子命令會影響**根**子命令。
  ## <a name="additional-references"></a>其他參考
  [命令列語法索引鍵](command-line-syntax-key.md)
  [nslookup root](nslookup-root.md)

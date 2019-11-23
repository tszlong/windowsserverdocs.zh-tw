---
title: nslookup root
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3eb3375df3a109685fc8dc5d23f0c5008339d09e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373386"
---
# <a name="nslookup-root"></a>nslookup root

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將伺服器的預設伺服器變更為網域名稱系統（DNS）功能變數名稱空間的根目錄。
## <a name="syntax"></a>語法
```
root 
```
## <a name="parameters"></a>Parameters

|    參數    |                      描述                      |
|-----------------|-------------------------------------------------------|
| {help &#124; ？} | 顯示**nslookup**子命令的簡短摘要。 |

## <a name="remarks"></a>備註
- 目前會使用 ns.nic.ddn.mil 名稱伺服器。 此命令是 lserver ns.nic.ddn.mil 的同義字。 您可以使用 [**設定根**] 命令來變更根功能變數名稱伺服器的名稱。
  ## <a name="additional-references"></a>其他參考
  [命令列語法索引鍵](command-line-syntax-key.md)
  [nslookup 集根](nslookup-set-root.md)

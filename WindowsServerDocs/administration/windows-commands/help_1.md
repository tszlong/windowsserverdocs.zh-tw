---
title: help
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75dbf94f-d79c-45b2-9463-c06648218f4a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c3d76a71ec287e5c874ae3e4dec34016c5c80336
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375576"
---
# <a name="help"></a>help

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示指定命令的可用命令或詳細說明資訊的清單。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
help [<command>]  
```  
  
## <a name="parameters"></a>參數  
  
| 參數 |                              描述                              |
|-----------|-----------------------------------------------------------------------|
| <command> | 指定要顯示其詳細說明資訊的命令。 |
  
## <a name="remarks"></a>備註  
  
-   如果未指定任何命令，[說明] 將會顯示所有**可能的命令**。  
  
## <a name="BKMK_examples"></a>典型  
若要顯示 DiskPart 中所有可用命令的清單，請輸入：  
  
```  
help  
```  
  
若要顯示有關如何在 DiskPart 中使用 [**建立資料分割主要**] 命令的詳細說明資訊，請輸入：  
  
```  
help create partition primary  
```  
  
#### <a name="additional-references"></a>其他參考  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  


---
title: help
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 29ec2da9d383985d62931ce1cee9bd4a83a0fb1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869589"
---
# <a name="help"></a>help

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在指定的命令，會顯示一份可用的命令或詳細的說明資訊。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
help [<command>]  
```  
  
## <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|<command>|指定要顯示詳細的說明資訊的命令。|  
  
## <a name="remarks"></a>備註  
  
-   如果未不指定任何命令，**協助**會顯示所有可能的命令。  
  
## <a name="BKMK_examples"></a>範例  
若要顯示 DiskPart 中可用的所有命令的清單，請輸入：  
  
```  
help  
```  
  
若要顯示詳細的說明如何使用資訊**建立資料分割的主要**類型 DiskPart 命令：  
  
```  
help create partition primary  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

  


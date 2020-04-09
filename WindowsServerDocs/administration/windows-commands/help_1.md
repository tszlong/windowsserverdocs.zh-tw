---
title: 說明
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75dbf94f-d79c-45b2-9463-c06648218f4a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 404cefe879e8f63678f2cba90516fad9c216eb23
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842311"
---
# <a name="help"></a>說明

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示指定命令的可用命令或詳細說明資訊的清單。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
help [<command>]  
```  
  
### <a name="parameters"></a>參數  
  
| 參數 |                              描述                              |
|-----------|-----------------------------------------------------------------------|
| <command> | 指定要顯示其詳細說明資訊的命令。 |
  
## <a name="remarks"></a>備註  
  
-   如果未指定任何命令，[說明] 將會顯示所有**可能的命令**。  
  
## <a name="examples"></a><a name=BKMK_examples></a>典型  
若要顯示 DiskPart 中所有可用命令的清單，請輸入：  
  
```  
help  
```  
  
若要顯示有關如何在 DiskPart 中使用 [**建立資料分割主要**] 命令的詳細說明資訊，請輸入：  
  
```  
help create partition primary  
```  
  
## <a name="additional-references"></a>其他參考資料  
- [命令列語法關鍵](command-line-syntax-key.md)  
  

  


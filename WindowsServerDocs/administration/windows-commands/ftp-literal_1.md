---
title: ftp literal_1
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 393dea27e8567a72a5bd25c927282ade93e317c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376268"
---
# <a name="ftp-literal_1"></a>ftp： literal_1

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 會將逐字引數傳送到遠端 ftp 伺服器。 會傳回單一 ftp 回復碼。   

## <a name="syntax"></a>語法  
```  
literal <Argument> [ ]  
```  
### <a name="parameters"></a>參數  

| 參數  |                    描述                    |
|------------|---------------------------------------------------|
| <Argument> | 指定要傳送到 ftp 伺服器的引數。 |

## <a name="remarks"></a>備註  
**常**值命令與**引號**命令相同。  
## <a name="BKMK_Examples"></a>典型  
將**quit**命令傳送到遠端 ftp 伺服器。  
```  
literal quit  
```  
## <a name="additional-references"></a>其他參考  
-   [ftp：引號](ftp-quote.md)  
-   [命令列語法關鍵](command-line-syntax-key.md)  

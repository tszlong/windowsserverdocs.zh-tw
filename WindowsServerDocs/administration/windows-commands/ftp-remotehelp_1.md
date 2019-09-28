---
title: ftp remotehelp_1
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bac6fbe4a55c3fed4caab4e30ba848ec9ea68e21
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376034"
---
# <a name="ftp-remotehelp_1"></a>ftp： remotehelp_1

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端命令的說明。   
## <a name="syntax"></a>語法  
```  
remotehelp [<Command>]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|[<Command>]|指定您想要協助的命令名稱。 如果未指定*Command* ， **ftp**會顯示所有遠端命令的清單。|  
## <a name="remarks"></a>備註  
您可以使用**引號**或**常**值來執行遠端命令。  
## <a name="BKMK_Examples"></a>典型  
顯示遠端命令的清單。  
```  
remotehelp  
```  
顯示 [**適用于] 遠端命令**的語法。  
```  
remotehelp feat  
```  
## <a name="additional-references"></a>其他參考  
-   [ftp：引號](ftp-quote.md)  
-   [命令列語法關鍵](command-line-syntax-key.md)  

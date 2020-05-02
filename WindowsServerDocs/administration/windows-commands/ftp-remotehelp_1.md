---
title: ftp remotehelp_1
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc4affb3f04eadaa4e0005e5edce0f564156f64a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725137"
---
# <a name="ftp-remotehelp_1"></a>ftp： remotehelp_1

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端命令的說明。   
## <a name="syntax"></a>語法  
```  
remotehelp [<Command>]  
```  
#### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|[<Command>]|指定您想要協助的命令名稱。 如果未指定*Command* ， **ftp**會顯示所有遠端命令的清單。|  
## <a name="remarks"></a>備註  
您可以使用**引號**或**常**值來執行遠端命令。  
## <a name="examples"></a>範例  
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
-   - [命令列語法關鍵](command-line-syntax-key.md)  

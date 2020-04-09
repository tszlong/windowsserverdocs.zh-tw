---
title: ftp remotehelp_1
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3c4a4ffec01fce5cde8b2aa9dd1fa0704f3a85ff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843051"
---
# <a name="ftp-remotehelp_1"></a>ftp： remotehelp_1

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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
## <a name="examples"></a><a name=BKMK_Examples></a>典型  
顯示遠端命令的清單。  
```  
remotehelp  
```  
顯示 [**適用于] 遠端命令**的語法。  
```  
remotehelp feat  
```  
## <a name="additional-references"></a>其他參考資料  
-   [ftp：引號](ftp-quote.md)  
-   - [命令列語法關鍵](command-line-syntax-key.md)  

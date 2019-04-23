---
title: ftp remotehelp_1
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bd64af157f7ce05330cdafe6e4db6787fa765859
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889589"
---
# <a name="ftp-remotehelp1"></a>ftp: remotehelp_1

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示說明遠端命令。   
## <a name="syntax"></a>語法  
```  
remotehelp [<Command>]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|[<Command>]|指定要說明的命令名稱。 如果*命令*未指定，則**ftp**會顯示一份所有遠端命令。|  
## <a name="remarks"></a>備註  
您可以執行遠端命令，使用**報價**或是**常值**。  
## <a name="BKMK_Examples"></a>範例  
顯示一份遠端命令。  
```  
remotehelp  
```  
顯示的語法**技巧**遠端命令。  
```  
remotehelp feat  
```  
## <a name="additional-references"></a>其他參考資料  
-   [ftp: quote](ftp-quote.md)  
-   [命令列語法關鍵](command-line-syntax-key.md)  

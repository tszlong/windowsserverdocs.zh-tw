---
title: ftp literal_1
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fc4f8aff5a22da93330a12a75e5f368285366216
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725251"
---
# <a name="ftp-literal_1"></a>ftp： literal_1

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 會將逐字引數傳送到遠端 ftp 伺服器。 會傳回單一 ftp 回復碼。   

## <a name="syntax"></a>語法  
```  
literal <Argument> [ ]  
```  
#### <a name="parameters"></a>參數  

| 參數  |                    描述                    |
|------------|---------------------------------------------------|
| <Argument> | 指定要傳送到 ftp 伺服器的引數。 |

## <a name="remarks"></a>備註  
**常**值命令與**引號**命令相同。  
## <a name="examples"></a>範例  
將**quit**命令傳送到遠端 ftp 伺服器。  
```  
literal quit  
```  
## <a name="additional-references"></a>其他參考  
-   [ftp：引號](ftp-quote.md)  
-   - [命令列語法關鍵](command-line-syntax-key.md)  

---
title: ftp 報價
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1101dd6a5fa163df8d43d182e9d0dfe66e340b60
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725145"
---
# <a name="ftp-quote"></a>ftp：引號

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將逐字引數傳送到遠端 ftp 伺服器。 會傳回單一 ftp 回復碼。   
## <a name="syntax"></a>語法  
```  
quote <Argument>[ ]  
```  
#### <a name="parameters"></a>參數  

| 參數  |                    描述                    |
|------------|---------------------------------------------------|
| <Argument> | 指定要傳送到 ftp 伺服器的引數。 |

## <a name="remarks"></a>備註  
[**引號**] 命令與 [**常**值] 命令相同。  
## <a name="examples"></a>範例  
將**quit**命令傳送到遠端 ftp 伺服器。  
```  
quote quit  
```  
## <a name="additional-references"></a>其他參考  
-   [ftp： literal_1](ftp-literal_1.md)  
-   - [命令列語法關鍵](command-line-syntax-key.md)  

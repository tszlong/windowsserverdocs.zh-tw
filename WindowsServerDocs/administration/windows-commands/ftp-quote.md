---
title: ftp 報價
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bf13704150d602fbfa4e3b1a3fb1774d3bf7363
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843031"
---
# <a name="ftp-quote"></a>ftp：引號

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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
## <a name="examples"></a><a name=BKMK_Examples></a>典型  
將**quit**命令傳送到遠端 ftp 伺服器。  
```  
quote quit  
```  
## <a name="additional-references"></a>其他參考資料  
-   [ftp： literal_1](ftp-literal_1.md)  
-   - [命令列語法關鍵](command-line-syntax-key.md)  

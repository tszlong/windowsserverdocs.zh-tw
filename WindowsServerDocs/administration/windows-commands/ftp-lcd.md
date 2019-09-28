---
title: ftp lcd
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47058cc62e4e87d1fcd951ec3b4a7885eeef2ae2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376315"
---
# <a name="ftp-lcd"></a>ftp： lcd

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更本機電腦上的工作目錄。 根據預設，工作目錄是**ftp**啟動所在的目錄。   
## <a name="syntax"></a>語法  
```  
lcd [<directory>]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|[<directory>]|指定要變更之本機電腦上的目錄。 如果未指定*目錄*，則目前的工作目錄會變更為預設目錄。|  
## <a name="BKMK_Examples"></a>典型  
將本機電腦上的工作目錄變更為**C:\dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>其他參考  
-   [命令列語法關鍵](command-line-syntax-key.md)  

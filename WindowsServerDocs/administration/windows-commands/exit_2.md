---
title: exit
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 23d1044b-f5c1-4180-ae6d-f553b48da4d9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6d3a4d0bfc74644f6fda43abe57e0e4e7c1264a
ms.sourcegitcommit: 4a03f263952c993dfdf339dd3491c73719854aba
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2019
ms.locfileid: "74791223"
---
# <a name="exit"></a>exit

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

結束 Cmd.exe 程式（命令直譯器）或目前的批次腳本。  
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。  
## <a name="syntax"></a>語法  
```  
exit [/b] [<exitCode>]  
```  
## <a name="parameters"></a>Parameters  

| 參數  |                                                                                         說明                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /b     |                                      結束目前的批次腳本，而不是結束 Cmd.exe。 如果是從批次腳本外部執行，則會結束 Cmd.exe。                                      |
| `<exitCode>` | 指定數位。 如果指定 **/b** ，ERRORLEVEL 環境變數會設定為該數位。 如果您要結束**cmd.exe**，進程結束代碼會設定為該數位。 |
|     /?     |                                                                             在命令提示字元顯示說明。                                                                             |

## <a name="BKMK_examples"></a>典型  
若要關閉命令直譯器，Cmd.exe，請輸入：  
```  
exit  
```  
## <a name="additional-references"></a>其他參考  
-   [命令列語法關鍵](command-line-syntax-key.md)  


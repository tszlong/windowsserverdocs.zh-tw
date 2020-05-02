---
title: exit
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23d1044b-f5c1-4180-ae6d-f553b48da4d9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e47dfa42f2bacb3fe9f12d1da9163bcf828e9488
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725699"
---
# <a name="exit"></a>exit

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

結束 Cmd.exe 程式（命令直譯器）或目前的批次腳本。  
  
## <a name="syntax"></a>語法  
```  
exit [/b] [<exitCode>]  
```  
### <a name="parameters"></a>參數  

| 參數  |                                                                                         描述                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /b     |                                      結束目前的批次腳本，而不是結束 Cmd.exe。 如果是從批次腳本外部執行，則會結束 Cmd.exe。                                      |
| `<exitCode>` | 指定數位。 如果指定 **/b** ，ERRORLEVEL 環境變數會設定為該數位。 如果您要結束**cmd.exe**，進程結束代碼會設定為該數位。 |
|     /?     |                                                                             在命令提示字元顯示說明。                                                                             |

## <a name="examples"></a>範例  
若要關閉命令直譯器，Cmd.exe，請輸入：  
```  
exit  
```  
## <a name="additional-references"></a>其他參考  
-   - [命令列語法關鍵](command-line-syntax-key.md)  


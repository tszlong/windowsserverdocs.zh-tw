---
title: exit
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4e599f84389b23e527e3718a620d5fdfefe24edb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439458"
---
# <a name="exit"></a>exit

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

結束 Cmd.exe 程式 （命令直譯器） 或目前的批次指令碼。  
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。  
## <a name="syntax"></a>語法  
```  
exit [/b] [<exitCode>]  
```  
## <a name="parameters"></a>參數  

| 參數  |                                                                                         描述                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /b     |                                      結束目前的批次指令碼，而不是結束 Cmd.exe。 如果從外部批次指令碼執行，就會結束 Cmd.exe。                                      |
| <exitCode> | 指定一個數字。 如果**b**指定，將 ERRORLEVEL 環境變數設定為該數字。 如果您已結束**Cmd.exe**，處理序結束碼設為該數字。 |
|     /?     |                                                                             在命令提示字元顯示說明。                                                                             |

## <a name="BKMK_examples"></a>範例  
若要關閉的命令直譯器，Cmd.exe，請輸入：  
```  
exit  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  


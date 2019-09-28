---
title: telnet 集合
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67316b5f-9c6f-43e3-86d5-dcff9ae2ac3e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e39e2812edc9cd5f169a046def26beebda1d007e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383616"
---
# <a name="telnet-set"></a>telnet：設定

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定選項。   
## <a name="syntax"></a>語法  
```  
set [bsasdel] [crlf] [delasbs] [escape <Char>] [localecho] [logfile <FileName>] [logging] [mode {console | stream}] [ntlm] [term {ansi | vt100 | vt52 | vtnt}] [?]  
```  
### <a name="parameters"></a>參數  

|                    參數                     |                                                                                                                                              描述                                                                                                                                              |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     bsasdel                      |                                                                                                                                 將**倒退鍵**當做**刪除**傳送。                                                                                                                                  |
|                       crlf                       |                                                                                                        當按下**Enter**鍵時，傳送 CR & LF （0x0D，0x 0A）。 所謂的新行模式。                                                                                                        |
|                     delasbs                      |                                                                                                                                 以**倒退鍵**的形式傳送**刪除**。                                                                                                                                  |
|                escape <Character>                | 設定用來輸入 telnet 用戶端提示的換用字元。 Escape 字元可以是單一字元，也可以是**CTRL**鍵加上一個字元的組合。 若要設定控制按鍵組合，請在輸入要指派的字元時按住**CTRL**鍵。 |
|                    localecho                     |                                                                                                                                         開啟本機 echo。                                                                                                                                          |
|                logfile <FileName>                |                                                                                               將目前的 telnet 會話記錄到本機檔案。 當您設定此選項時，會自動開始記錄。                                                                                               |
|                     logging                      |                                                                                                                  開啟記錄功能。 如果未設定任何記錄檔，則會出現錯誤訊息。                                                                                                                   |
|           模式 {主控台&#124;畫面}           |                                                                                                                                       設定作業模式。                                                                                                                                        |
|                       ntlm                       |                                                                                                                                     開啟 NTLM 驗證。                                                                                                                                     |
| 詞彙 {ansi &#124; vt100 &#124; vt52 &#124; vtnt} |                                                                                                                                        設定終端機類型。                                                                                                                                        |
|                        ?                         |                                                                                                                                    顯示此命令的說明。                                                                                                                                    |

## <a name="remarks"></a>備註  
1. 您可以使用 [取消設定] 命令來關閉先前**設定的選項**。  
2. 在非英文版的 telnet 上，可以使用**codeset** <option>。 **Codeset** <option> 會將目前的程式碼設定為選項，可以是下列任何一項： **shift JIS**、**日文 EUC**、 **jis 漢字**、 **jis 漢字（78）** 、 **DEC 漢字**、 **NEC 漢字**。 您應該在遠端電腦上設定相同的程式碼。  
   ## <a name="BKMK_Examples"></a>典型  
   設定記錄檔，並開始記錄到本機檔案 tnlog .txt  
   ```  
   set logfile tnlog.txt  
   ```  
   ## <a name="additional-references"></a>其他參考  
3. [命令列語法關鍵](command-line-syntax-key.md)  

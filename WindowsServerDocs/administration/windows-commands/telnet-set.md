---
title: telnet 組
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b68ce0ee87d80b25cf13db5bebc6c407a9fe091f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441026"
---
# <a name="telnet-set"></a>telnet： 設定

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

設定選項。   
## <a name="syntax"></a>語法  
```  
set [bsasdel] [crlf] [delasbs] [escape <Char>] [localecho] [logfile <FileName>] [logging] [mode {console | stream}] [ntlm] [term {ansi | vt100 | vt52 | vtnt}] [?]  
```  
### <a name="parameters"></a>參數  

|                    參數                     |                                                                                                                                              描述                                                                                                                                              |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     bsasdel                      |                                                                                                                                 傳送**退格鍵**作為**刪除**。                                                                                                                                  |
|                       crlf                       |                                                                                                        傳送 CR 和 LF (0x0D，0x0a) 時**Enter**按下按鍵。 所謂的新行模式。                                                                                                        |
|                     delasbs                      |                                                                                                                                 傳送**刪除**作為**退格鍵**。                                                                                                                                  |
|                逸出 <Character>                | 設定用來輸入 telnet 用戶端的命令提示字元的逸出字元。 逸出字元可以是單一字元，或它可以是組成**CTRL**再加上字元的索引鍵。 若要設定控制項索引鍵組合，按住**CTRL**金鑰時輸入您想要指派的字元。 |
|                    localecho                     |                                                                                                                                         開啟本機回應。                                                                                                                                          |
|                記錄檔 <FileName>                |                                                                                               本機檔案記錄目前 telnet 工作階段。 當您將此選項時記錄會自動開始。                                                                                               |
|                     logging                      |                                                                                                                  開啟的記錄。 如果沒有記錄檔設定時，便會出現錯誤訊息。                                                                                                                   |
|           模式 {主控台&#124;畫面}           |                                                                                                                                       設定作業模式。                                                                                                                                        |
|                       Ntlm                       |                                                                                                                                     開啟 NTLM 驗證。                                                                                                                                     |
| 詞彙 {ansi &#124; vt100 &#124; vt52 &#124; vtnt} |                                                                                                                                        設定終端機類型。                                                                                                                                        |
|                        ?                         |                                                                                                                                    此命令會顯示說明。                                                                                                                                    |

## <a name="remarks"></a>備註  
1. 您可以使用**unset**命令來關閉先前設定的選項。  
2. 在非英文版的 telnet**代碼**<option>可用。 **代碼**<option>設定目前的程式碼設定的選項，可以是下列任一項： **shift JIS**，**日文 EUC**， **JIS 漢字**，**JIS 漢字 (78)** ， **DEC 漢字**， **NEC 漢字**。 您應該設定為遠端電腦上設定相同的程式碼。  
   ## <a name="BKMK_Examples"></a>範例  
   設定記錄檔，並開始記錄到本機檔案 tnlog.txt  
   ```  
   set logfile tnlog.txt  
   ```  
   ## <a name="additional-references"></a>其他參考資料  
3. [命令列語法關鍵](command-line-syntax-key.md)  

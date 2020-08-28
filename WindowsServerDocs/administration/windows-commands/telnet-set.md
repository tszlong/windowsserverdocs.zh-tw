---
title: telnet set
description: Telnet set 的參考文章，可設定選項。
ms.topic: reference
ms.assetid: 67316b5f-9c6f-43e3-86d5-dcff9ae2ac3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 90b25b24da8af743d6e027bd26c2de7f155544b6
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038346"
---
# <a name="telnet-set"></a>telnet：設定

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定選項。

## <a name="syntax"></a>語法
```
set [bsasdel] [crlf] [delasbs] [escape <Char>] [localecho] [logfile <FileName>] [logging] [mode {console | stream}] [ntlm] [term {ansi | vt100 | vt52 | vtnt}] [?]
```
#### <a name="parameters"></a>參數

|                    參數                     |                                                                                                                                              描述                                                                                                                                              |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     bsasdel                      |                                                                                                                                 將 **倒退鍵** 傳送給 **刪除**。                                                                                                                                  |
|                       crlf                       |                                                                                                        在按下 **Enter** 鍵時，將 CR & LF (0x0D，0x 0A) 。 稱為新行模式。                                                                                                        |
|                     delasbs                      |                                                                                                                                 以**倒退鍵**的形式傳送**刪除**。                                                                                                                                  |
|                逃脫 <Character>                | 設定用來輸入 telnet 用戶端提示的 escape 字元。 Escape 字元可以是單一字元，也可以是 **CTRL** 鍵加上字元的組合。 若要設定控制按鍵組合，請按住 **CTRL** 鍵，同時輸入您要指派的字元。 |
|                    localecho                     |                                                                                                                                         開啟本機 echo。                                                                                                                                          |
|                記錄檔 <FileName>                |                                                                                               將目前的 telnet 會話記錄到本機檔案。 當您設定這個選項的同時便會自動開始記錄。                                                                                               |
|                     logging                      |                                                                                                                  開啟記錄。 如果未設定任何記錄檔，則會顯示錯誤訊息。                                                                                                                   |
|           模式 {主控台 &#124; 畫面}           |                                                                                                                                       設定作業模式。                                                                                                                                        |
|                       ntlm                       |                                                                                                                                     開啟 NTLM 驗證。                                                                                                                                     |
| 詞彙 {ansi &#124; vt100 &#124; vt52 &#124; vtnt} |                                                                                                                                        設定終端機類型。                                                                                                                                        |
|                        ?                         |                                                                                                                                    顯示此命令的說明。                                                                                                                                    |

## <a name="remarks"></a>備註
1. 您可以 **使用 [取消** 設定] 命令來關閉先前設定的選項。
2. 在非英文版的 telnet 上， **codeset** <option> 可以使用 codeset。 **Codeset** <option> 將目前的程式碼集設定為選項，它可以是下列任一項： **shift-jis**、 **日文 EUC**、 **JIS 漢字**、 **jis 漢字 (78) **、 **DEC**漢字、NEC 漢字、 **NEC**漢字。 您應該在遠端電腦上設定相同的程式碼。
   ## <a name="examples"></a>範例
   設定記錄檔並開始記錄到本機檔案 tnlog.txt
   ```
   set logfile tnlog.txt
   ```
   ## <a name="additional-references"></a>其他參考資料
3. - [命令列語法關鍵](command-line-syntax-key.md)

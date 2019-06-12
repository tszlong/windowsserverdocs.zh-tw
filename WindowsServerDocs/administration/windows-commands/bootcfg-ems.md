---
title: bootcfg ems
description: 適用於 Windows 命令主題**bootcfg ems** -可讓使用者加入或變更遠端電腦的 Emergency Management Services 主控台重新導向的設定。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57abdc50-c64a-45f1-8470-3f8c3a51f743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d971fab552b321d54899b418eac4f54c4665ccb7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434744"
---
# <a name="bootcfg-ems"></a>bootcfg ems

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

可讓使用者加入或變更遠端電腦的 Emergency Management Services 主控台重新導向的設定。 藉由啟用緊急管理服務，您將新增 「 重新導向 = 連接埠號碼 」 列 Boot.ini 檔案和指定的作業系統項目線條的遠端支援選項的 [開機載入器] 一節。 只能在伺服器上啟用緊急管理服務功能。

## <a name="syntax"></a>語法
```
bootcfg /ems {ON | OFF | edit} [/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>參數

|                            參數                             |                                                                                                                                                                                                                                                                                                                                                              描述                                                                                                                                                                                                                                                                                                                                                              |
|------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    {ON &#124; OFF&#124; edit}                    | 指定緊急管理服務的重新導向的值。<br /><br />**ON** -可讓指定的遠端輸出<OSEntryLineNum>。 加入至指定的遠端支援選項<OSEntryLineNum>和 重新導向 = com<X>設為 [開機載入器] 區段。 值的 com<X>由設定 **/連接埠**參數。<br /><br />**關閉**-停用遠端電腦的輸出。 從指定中移除遠端支援選項<OSEntryLineNum>和重新導向 = com<X>設定從 [開機載入器] 區段。<br /><br />**編輯**-允許藉由變更重新導向的通訊埠設定變更 = com<X> [開機載入器] 區段中設定。 值的 com<X>重設為所指定的值 **/連接埠**參數。 |
|                          /s <computer>                           |                                                                                                                                                                                                                                                                                                          指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。                                                                                                                                                                                                                                                                                                           |
|                       /u <Domain>\\<User>                        |                                                                                                                                                                                                                                                                 使用指定的使用者帳戶權限執行命令<User>或是<Domain> \\ <User>。 預設值是目前登入的使用者發出命令的電腦上的權限。                                                                                                                                                                                                                                                                  |
|                          /p <Password>                           |                                                                                                                                                                                                                                                                                                                         指定在指定的使用者帳戶的密碼 **/u**參數。                                                                                                                                                                                                                                                                                                                         |
| /port {COM1 &#124; COM2 &#124; COM3 &#124; COM4 &#124; BIOSSET}  |                                                                                                                                                                                                                              指定要用於重新導向的 COM 連接埠。 **BIOSSET**指示緊急管理服務，以取得 BIOS 設定，以決定應使用哪個連接埠重新導向。 請勿使用 **/連接埠**被停用遠端管理的輸出參數。                                                                                                                                                                                                                              |
| /baud {9600 &#124; 19200 &#124; 38400&#124; 57600 &#124; 115200} |                                                                                                                                                                                                                                                                                               指定要用於重新導向的傳輸速率。 請勿使用 **/baud**被停用遠端管理的輸出參數。                                                                                                                                                                                                                                                                                               |
|                       /id <OSEntryLineNum>                       |                                                                                                                                                                                              指定的緊急管理服務選項加入 Boot.ini 檔案的 [operating systems] 區段中的作業系統項目行號。 [Operating systems] 區段標頭之後的第一行會是 1。 這個參數時，必須緊急管理服務的值設定為**ON**或是**OFF**。                                                                                                                                                                                              |
|                                /?                                |                                                                                                                                                                                                                                                                                                                                                 在命令提示字元顯示說明。                                                                                                                                                                                                                                                                                                                                                  |

## <a name="BKMK_examples"></a>範例
下列範例示範如何使用**bootcfg /ems**命令：
```
bootcfg /ems on /port com1 /baud 19200 /id 2 
bootcfg /ems on /port biosset /id 3 
bootcfg /s srvmain /ems off /id 2 
bootcfg /ems edit /port com2 /baud 115200 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /ems off /id 2
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)

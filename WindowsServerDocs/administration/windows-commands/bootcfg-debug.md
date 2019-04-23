---
title: bootcfg debug
description: 適用於 Windows 命令主題**bootcfg 偵錯**-新增或變更偵錯設定為指定的作業系統項目。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 28afa5fb-a236-46e2-b1a4-a3c43a49c437
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27768788e7f14445137331523c62151fcf3b6b2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879019"
---
# <a name="bootcfg-debug"></a>bootcfg debug

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

加入或變更偵錯設定為指定的作業系統項目。

## <a name="syntax"></a>語法
```
bootcfg /debug {ON | OFF | edit}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|{ON &#124; OFF&#124; edit}|指定偵錯的值。<br /><br />**ON** -藉由將加入指定 /debug 選項會啟用遠端偵錯支援<OSEntryLineNum>。<br /><br />**關閉**-停用遠端偵錯的支援，藉由移除 /debug 選項，從指定<OSEntryLineNum>。<br /><br />**編輯**-藉由變更指定 /debug 選項相關聯的值來允許對連接埠和傳輸速率設定<OSEntryLineNum>。|
|/s <computer>|指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。|
|/u <Domain>\\<User>|使用指定的使用者帳戶權限執行命令<User>或是<Domain> \\ <User>。 預設值是目前登入的使用者發出命令的電腦上的權限。|
|/p <Password>|指定在指定的使用者帳戶的密碼 **/u**參數。|
|/port {COM1 &#124; COM2 &#124; COM3 &#124; COM4}|指定要用於偵錯的 COM 連接埠。 請勿使用 **/連接埠**參數如果偵錯已停用。|
|/baud {9600&#124; 19200&#124; 38400&#124; 57600&#124; 115200}|指定要偵錯要使用的傳輸速率。 請勿使用 **/baud**參數如果偵錯已停用。|
|/id <OSEntryLineNum>|偵錯的選項新增到其中的 Boot.ini 檔案的 [operating systems] 區段中指定的作業系統項目行號。 [Operating systems] 區段標頭之後的第一行會是 1。|
|/?|在命令提示字元顯示說明。|
##### <a name="remarks"></a>備註
-   如果需要 1394年連接埠偵錯功能，使用[bootcfg dbg1394](bootcfg-dbg1394.md)。
## <a name="BKMK_examples"></a>範例
下列範例示範如何使用**bootcfg /debug**命令：
```
bootcfg /debug on /port com1 /id 2 
bootcfg /debug edit /port com2 /baud 19200 /id 2 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)

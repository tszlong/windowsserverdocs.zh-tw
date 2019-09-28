---
title: bootcfg debug
description: 適用于**bootcfg debug**的 Windows 命令主題-加入或變更指定之作業系統專案的偵錯工具設定。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f6659cf2bfdf83b1b2fe6f6c811365775526768a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380049"
---
# <a name="bootcfg-debug"></a>bootcfg debug

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

加入或變更指定之作業系統專案的偵錯工具設定。

## <a name="syntax"></a>語法
```
bootcfg /debug {ON | OFF | edit}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>參數

|                           參數                           |                                                                                                                                                                                                                    描述                                                                                                                                                                                                                    |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  {ON &#124; OFF&#124; edit}                   | 指定用於偵錯工具的值。<br /><br />**ON** -藉由將/debug 選項新增至指定的 <OSEntryLineNum>，啟用遠端偵錯程式支援。<br /><br />**OFF** -從指定的 <OSEntryLineNum> 移除/debug 選項，以停用遠端偵錯程式支援。<br /><br />**編輯**-藉由變更指定 <OSEntryLineNum> 的/debug 選項相關聯的值，允許變更埠和傳輸速率設定。 |
|                         /s <computer>                         |                                                                                                                                                                指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。                                                                                                                                                                 |
|                      /u <Domain> @ no__t-1 @ no__t-2                      |                                                                                                                       以 <User> 或 <Domain> @ no__t-2 @ no__t-3 指定之使用者的帳戶許可權來執行命令。 預設為發出命令之電腦上目前登入使用者的許可權。                                                                                                                        |
|                         /p <Password>                         |                                                                                                                                                                               指定 **/u**參數中指定之使用者帳戶的密碼。                                                                                                                                                                               |
|       /port {COM1 &#124; COM2 &#124; COM3 &#124; COM4}        |                                                                                                                                                                指定要用於進行偵錯工具的 COM 埠。 如果停用調試功能，請勿使用 **/port**參數。                                                                                                                                                                |
| /baud {9600&#124; 19200&#124; 38400&#124; 57600&#124; 115200} |                                                                                                                                                               指定用於進行偵錯工具的傳輸速率。 如果停用調試功能，請勿使用 **/baud**參數。                                                                                                                                                                |
|                     /id <OSEntryLineNum>                      |                                                                                                               在要加入偵錯工具之 Boot.ini 檔案的 [作業系統] 區段中，指定作業系統專案行號。 [作業系統] 區段標頭後面的第一行是1。                                                                                                                |
|                              /?                               |                                                                                                                                                                                                       在命令提示字元顯示說明。                                                                                                                                                                                                        |

##### <a name="remarks"></a>備註
- 如果需要進行1394埠調試，請使用[bootcfg dbg1394](bootcfg-dbg1394.md)。
  ## <a name="BKMK_examples"></a>典型
  下列範例會示範如何使用**bootcfg/debug**命令：
  ```
  bootcfg /debug on /port com1 /id 2 
  bootcfg /debug edit /port com2 /baud 19200 /id 2 
  bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
  ```
  #### <a name="additional-references"></a>其他參考
  [命令列語法關鍵](command-line-syntax-key.md)

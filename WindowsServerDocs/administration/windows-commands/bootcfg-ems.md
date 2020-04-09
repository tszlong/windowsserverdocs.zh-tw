---
title: bootcfg ems
description: 適用于 bootcfg ems 的 Windows 命令主題，可讓使用者新增或變更緊急管理服務主控台重新導向至遠端電腦的設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57abdc50-c64a-45f1-8470-3f8c3a51f743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 825426b3ce865f96892f8a8d9b11f11650e1b6c1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848531"
---
# <a name="bootcfg-ems"></a>bootcfg ems

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓使用者新增或變更緊急管理服務主控台重新導向至遠端電腦的設定。 藉由啟用緊急管理服務，您可以將 [重新導向] = 埠 # 行新增至 Boot.ini 檔案的 [開機載入器] 區段，並將/redirect 選項加入至指定的作業系統專案行。 [緊急管理服務] 功能只會在伺服器上啟用。

## <a name="syntax"></a>語法
```
bootcfg /ems {ON | OFF | edit} [/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
### <a name="parameters"></a>參數

|                            參數                             |                                                                                                                                                                                                                                                                                                                                                              描述                                                                                                                                                                                                                                                                                                                                                              |
|------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    {ON &#124; OFF&#124; edit}                    | 指定緊急管理服務重新導向的值。<p>**ON** -針對指定的 <OSEntryLineNum>啟用遠端輸出。 將/redirect 選項加入至指定的 <OSEntryLineNum>，並將 重新導向 = com<X> 設定新增至 [開機載入器] 區段。 Com<X> 的值是由 **/port**參數所設定。<p>**OFF** -停用輸出至遠端電腦。 從指定的 <OSEntryLineNum> 和 [開機載入器] 區段中的 [重新導向 = com<X>] 設定移除/redirect 選項。<p>**編輯**-藉由變更 [開機載入器] 區段中的 [重新導向 = com<X>] 設定，允許變更埠設定。 Com<X> 的值會重設為 **/port**參數所指定的值。 |
|                          /s <computer>                           |                                                                                                                                                                                                                                                                                                          指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。                                                                                                                                                                                                                                                                                                           |
|                       /u <Domain>\\<User>                        |                                                                                                                                                                                                                                                                 以 <User> 或 <Domain>\\<User>所指定使用者的帳戶許可權來執行命令。 預設為發出命令之電腦上目前登入使用者的許可權。                                                                                                                                                                                                                                                                  |
|                          /p <Password>                           |                                                                                                                                                                                                                                                                                                                         指定 **/u**參數中指定之使用者帳戶的密碼。                                                                                                                                                                                                                                                                                                                         |
| /port {COM1 &#124; COM2 &#124; COM3 &#124; COM4 &#124; BIOSSET}  |                                                                                                                                                                                                                              指定要用於重新導向的 COM 埠。 **BIOSSET**會指示緊急管理服務來取得 BIOS 設定，以判斷應該使用哪一個埠進行重新導向。 如果要停用遠端系統管理的輸出，請勿使用 **/port**參數。                                                                                                                                                                                                                              |
| /baud {9600 &#124; 19200 &#124; 38400&#124; 57600 &#124; 115200} |                                                                                                                                                                                                                                                                                               指定要用於重新導向的傳輸速率。 如果要停用遠端系統管理的輸出，請勿使用 **/baud**參數。                                                                                                                                                                                                                                                                                               |
|                       /id <OSEntryLineNum>                       |                                                                                                                                                                                              指定在 Boot.ini 檔案的 [作業系統] 區段中新增 [緊急管理服務] 選項的作業系統專案行號。 [作業系統] 區段標頭後面的第一行是1。 當 [緊急管理服務] 值設定為 [**開啟**] 或 [**關閉**] 時，此為必要參數。                                                                                                                                                                                              |
|                                /?                                |                                                                                                                                                                                                                                                                                                                                                 在命令提示字元顯示說明。                                                                                                                                                                                                                                                                                                                                                  |

## <a name="examples"></a><a name=BKMK_examples></a>典型
下列範例會示範如何使用**bootcfg/ems**命令：
```
bootcfg /ems on /port com1 /baud 19200 /id 2 
bootcfg /ems on /port biosset /id 3 
bootcfg /s srvmain /ems off /id 2 
bootcfg /ems edit /port com2 /baud 115200 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /ems off /id 2
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)

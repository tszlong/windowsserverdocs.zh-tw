---
title: bootcfg dbg1394
description: 適用於 Windows 命令主題**bootcfg dbg1394** -設定 1394年連接埠偵錯指定的作業系統項目
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35724697-90dd-4dbe-85b0-337fbd369dcc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85a554e25d1553ea4cd9415bb180df4751966926
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434841"
---
# <a name="bootcfg-dbg1394"></a>bootcfg dbg1394

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

設定 1394年連接埠的偵錯指定的作業系統項目。

## <a name="syntax"></a>語法
```
bootcfg /dbg1394 {ON | OFF}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/ch <Channel>] /id <OSEntryLineNum>
```
## <a name="parameters"></a>參數

|      參數       |                                                                                                                                           描述                                                                                                                                            |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   {ON &#124; OFF}    | 指定偵錯的 1394年連接埠的值。<br /><br />-   **ON** -啟用遠端偵錯的支援加入至指定的 /dbg1394 選項<OSEntryLineNum>。<br />-   **關閉**-停用遠端偵錯的支援，藉由移除 /dbg1394 選項，從指定<OSEntryLineNum>。 |
|    /s <computer>     |                                                                                        指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。                                                                                        |
| /u <Domain>\\<User>  |                                               使用指定的使用者帳戶權限執行命令<User>或是<Domain> \\ <User>。 預設值是目前登入的使用者發出命令的電腦上的權限。                                               |
|    /p <Password>     |                                                                                                      指定在指定的使用者帳戶的密碼 **/u**參數。                                                                                                       |
|     /ch 通道      |                                                           指定要用於偵錯通道。 有效值為 1 到 64 之間的整數。 請勿使用 **/ch** <Channel>如果 1394年連接埠偵錯已停用的參數。                                                           |
| /id <OSEntryLineNum> |                                  Boot.ini 檔案新增到其中的 1394年連接埠偵錯選項的 [operating systems] 區段中指定的作業系統項目行號。 [Operating systems] 區段標頭之後的第一行會是 1。                                  |
|          /?          |                                                                                                                               在命令提示字元顯示說明。                                                                                                                               |

## <a name="BKMK_examples"></a>範例
下列範例示範如何使用**bootcfg /dbg1394**命令：
```
bootcfg /dbg1394 /id 2 
bootcfg /dbg1394 on /ch 1 /id 3 
bootcfg /dbg1394 edit /ch 8 /id 2 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /dbg1394 off /id 2
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)

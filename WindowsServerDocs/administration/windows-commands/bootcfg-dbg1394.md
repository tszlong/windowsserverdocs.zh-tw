---
title: bootcfg dbg1394
description: 適用于**bootcfg dbg1394**的 Windows 命令主題-為指定的作業系統專案設定1394埠的偵錯工具
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8550871c60343fdc6d797f3f81729c24270400b4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380094"
---
# <a name="bootcfg-dbg1394"></a>bootcfg dbg1394

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

為指定的作業系統專案設定1394埠的偵錯工具。

## <a name="syntax"></a>語法
```
bootcfg /dbg1394 {ON | OFF}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/ch <Channel>] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parameters

|      參數       |                                                                                                                                           描述                                                                                                                                            |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   {ON &#124; OFF}    | 指定1394埠偵錯工具的值。<br /><br />-   **ON** -藉由將/dbg1394 選項加入至指定的 <OSEntryLineNum>，啟用遠端偵錯程式支援。<br />-   **OFF** -從指定的 <OSEntryLineNum>移除/dbg1394 選項，以停用遠端偵錯程式支援。 |
|    /s <computer>     |                                                                                        指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。                                                                                        |
| /u <Domain>\\<User>  |                                               以 <User> 或 <Domain>\\<User>所指定使用者的帳戶許可權來執行命令。 預設為發出命令之電腦上目前登入使用者的許可權。                                               |
|    /p <Password>     |                                                                                                      指定 **/u**參數中指定之使用者帳戶的密碼。                                                                                                       |
|     /ch 通道      |                                                           指定用於進行偵錯工具的通道。 有效值是介於1到64之間的整數。 如果要停用1394埠調試功能，請勿使用 **/ch** <Channel> 參數。                                                           |
| /id <OSEntryLineNum> |                                  在要新增1394埠偵錯工具的 Boot.ini 檔案的 [作業系統] 區段中，指定作業系統專案行號。 [作業系統] 區段標頭後面的第一行是1。                                  |
|          /?          |                                                                                                                               在命令提示字元顯示說明。                                                                                                                               |

## <a name="BKMK_examples"></a>典型
下列範例會示範如何使用**bootcfg/dbg1394**命令：
```
bootcfg /dbg1394 /id 2 
bootcfg /dbg1394 on /ch 1 /id 3 
bootcfg /dbg1394 edit /ch 8 /id 2 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /dbg1394 off /id 2
```
#### <a name="additional-references"></a>其他參考
[命令列語法關鍵](command-line-syntax-key.md)

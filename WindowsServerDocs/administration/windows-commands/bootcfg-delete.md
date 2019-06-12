---
title: bootcfg delete
description: 適用於 Windows 命令主題**bootcfg 刪除**-刪除 Boot.ini 檔案 [operating systems] 部分中的作業系統項目。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 71382e29-9b39-41c8-9c23-cf0ff829440a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37f30a181402fe8a74148b42398641af3c4846b9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434761"
---
# <a name="bootcfg-delete"></a>bootcfg delete

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

刪除 [operating systems] 一節的 Boot.ini 檔案中的作業系統項目。

## <a name="syntax"></a>語法
```
bootcfg /delete [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>參數

|         詞彙         |                                                                                             定義                                                                                              |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。                                          |
| /u <Domain>\\<User>  | 使用指定的使用者帳戶權限執行命令<User>或是<Domain> \\ <User>。 預設值是目前登入的使用者發出命令的電腦上的權限。 |
|    /p <Password>     |                                                        指定在指定的使用者帳戶的密碼 **/u**參數。                                                        |
| /id <OSEntryLineNum> |        要刪除的 Boot.ini 檔案的 [operating systems] 區段中指定的作業系統項目行號。 [Operating systems] 區段標頭之後的第一行會是 1。        |
|          /?          |                                                                                在命令提示字元顯示說明。                                                                                 |

## <a name="BKMK_examples"></a>範例
下列範例示範如何使用**bootcfg /delete**命令：
```
bootcfg /delete /id 1
bootcfg /delete /s srvmain /u maindom\hiropln /p p@ssW23 /id 3
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)

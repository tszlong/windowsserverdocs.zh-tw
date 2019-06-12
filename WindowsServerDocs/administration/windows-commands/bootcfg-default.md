---
title: bootcfg default
description: 適用於 Windows 命令主題**bootcfg 預設**-指定將指定為預設的作業系統項目。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e21824d7-8278-41d7-a2c5-ce09803d513a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63aaa7a044634d29c61f3085b1f0c015f4e64444
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434836"
---
# <a name="bootcfg-default"></a>bootcfg default

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

指定將指定為預設的作業系統項目。

## <a name="syntax"></a>語法
```
bootcfg /default [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>參數

|      參數       |                                                                                             描述                                                                                              |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                          指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。                                          |
| /u <Domain>\\<User>  | 使用指定的使用者帳戶權限執行命令<User>或是<Domain> \\ <User>。 預設值是目前登入的使用者發出命令的電腦上的權限。 |
|    /p <Password>     |                                                        指定在指定的使用者帳戶的密碼 **/u**參數。                                                         |
| /id <OSEntryLineNum> | Boot.ini 檔案，將指定為預設的 [operating systems] 區段中指定的作業系統項目行號。 [Operating systems] 區段標頭之後的第一行會是 1。  |
|          /?          |                                                                                 在命令提示字元顯示說明。                                                                                 |

## <a name="BKMK_examples"></a>範例
下列範例示範如何使用**bootcfg /default**命令：
```
bootcfg /default /id 2
bootcfg /default /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)

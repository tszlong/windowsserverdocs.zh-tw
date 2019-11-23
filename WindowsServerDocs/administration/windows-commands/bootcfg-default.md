---
title: bootcfg default
description: 適用于**bootcfg default**的 Windows 命令主題-指定要指定為預設值的作業系統專案。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e69868739a9c338b711984ba0f03452f307b430b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380020"
---
# <a name="bootcfg-default"></a>bootcfg default

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

指定要指定為預設值的作業系統專案。

## <a name="syntax"></a>語法
```
bootcfg /default [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parameters

|      參數       |                                                                                             描述                                                                                              |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                          指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。                                          |
| /u <Domain>\\<User>  | 以 <User> 或 <Domain>\\<User>所指定使用者的帳戶許可權來執行命令。 預設為發出命令之電腦上目前登入使用者的許可權。 |
|    /p <Password>     |                                                        指定 **/u**參數中指定之使用者帳戶的密碼。                                                         |
| /id <OSEntryLineNum> | 在 Boot.ini 檔案的 [作業系統] 區段中，指定要指定為預設值的作業系統專案行號。 [作業系統] 區段標頭後面的第一行是1。  |
|          /?          |                                                                                 在命令提示字元顯示說明。                                                                                 |

## <a name="BKMK_examples"></a>典型
下列範例會示範如何使用**bootcfg/default**命令：
```
bootcfg /default /id 2
bootcfg /default /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
#### <a name="additional-references"></a>其他參考
[命令列語法關鍵](command-line-syntax-key.md)

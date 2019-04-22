---
title: bootcfg copy
description: 適用於 Windows 命令主題**bootcfg 複製**-會建立一份現有的開機項目，您可以新增命令列選項。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2a236c2a-8675-444d-b695-9cbc9aff643b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff8cf2751b229c6358e240444de940658f2eb2fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825509"
---
# <a name="bootcfg-copy"></a>bootcfg copy

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

會建立一份現有的開機項目，您可以新增命令列選項。

## <a name="syntax"></a>語法
```
bootcfg /copy [/s <computer> [/u <Domain>\<User> /p <Password>]] [/d <Description>] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/s <computer>|指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。|
|/u <Domain>\\<User>|使用指定的使用者帳戶權限執行命令<User>或是<Domain> \\ <User>。 預設值是目前登入的使用者發出命令的電腦上的權限。|
|/p <Password>|指定在指定的使用者帳戶的密碼 **/u**參數。|
|/d <Description>|指定新的作業系統項目描述。|
|/id <OSEntryLineNum>|要複製的 Boot.ini 檔案的 [operating systems] 區段中指定的作業系統項目行號。 [Operating systems] 區段標頭之後的第一行會是 1。|
|/?|在命令提示字元顯示說明。|
## <a name="BKMK_examples"></a>範例
下列範例示範如何使用**bootcfg /copy**命令來複製 1 的開機項目和輸入"\ABC Server\\"做為的描述：
```
bootcfg /copy /d "\ABC Server\" /id 1
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)

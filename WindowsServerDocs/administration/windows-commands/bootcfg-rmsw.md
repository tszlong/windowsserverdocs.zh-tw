---
title: bootcfg rmsw
description: 適用於 Windows 命令主題**bootcfg rmsw** -移除操作系統載入選項，針對指定的作業系統項目。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fd7e4248-880e-4e2b-929e-87f8d44b9a63
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65a8ba452911eb1b46a748d4e9d639ed102421b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878819"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

移除作業系統載入選項，針對指定的作業系統項目。

## <a name="syntax"></a>語法
```
bootcfg /rmsw [/s <computer> [/u <Domain>\<User> [/p <Password>]]] [/mm] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/s <computer>|指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。|
|/u <Domain>\\<User>|使用指定的使用者帳戶權限執行命令<User>或是<Domain> \\ <User>。 預設值是目前登入的使用者發出命令的電腦上的權限。|
|/p <Password>|指定在指定的使用者帳戶的密碼 **/u**參數。|
|/mm|移除 /maxmem 選項和其相關聯的最大記憶體值從指定<OSEntryLineNum>。 /Maxmem 選項指定的最大作業系統可用的 RAM 數量。|
|/bv|從指定移除 /basevideo 選項<OSEntryLineNum>。 /Basevideo 選項會引導您使用標準的 VGA 模式來安裝的視訊驅動程式的作業系統。|
|/so|從指定移除 /sos 選項<OSEntryLineNum>。 /Sos 選項會指示要顯示正在載入裝置驅動程式名稱的作業系統。|
|/ng|從指定移除 /noguiboot 選項<OSEntryLineNum>。 /Noguiboot 選項會停用之前 CTRL + ALT + del 登入提示會顯示進度列。|
|/id <OSEntryLineNum>|要從中移除 OS 載入選項的 Boot.ini 檔案的 [operating systems] 區段中指定的作業系統項目行號。 [Operating systems] 區段標頭之後的第一行會是 1。|
|/?|在命令提示字元顯示說明。|
## <a name="BKMK_examples"></a>範例
下列範例示範如何使用**bootcfg /rmsw**命令：
```
bootcfg /rmsw /mm 64 /id 2 
bootcfg /rmsw /so /id 3 
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /rmsw /ng /id 2 
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2       
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)

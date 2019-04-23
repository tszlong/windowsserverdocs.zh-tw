---
title: bootcfg addsw
description: 適用於 Windows 命令主題**bootcfg addsw** -新增作業系統載入選項，可針對指定的作業系統項目。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 768e9c5bcf8a5d272927d013ff4accf5c3237219
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862869"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

新增作業系統載入選項，可針對指定的作業系統項目。

## <a name="syntax"></a>語法
```
bootcfg /addsw [/s <computer> [/u <Domain>\<User> /p <Password>]] [/mm <MaximumRAM>] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>參數
|詞彙|定義|
|----|-------|
|/s <computer>|指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。|
|/u <Domain>\\<User>|使用指定的使用者帳戶權限執行命令<User>或是<Domain> \\ <User>。 預設值是目前登入的使用者發出命令的電腦上的權限。|
|/p <Password>|指定在指定的使用者帳戶的密碼 **/u**參數。|
|/mm <MaximumRAM>|指定最大的 RAM 數量，以 mb 為單位，可以使用的作業系統。 值必須等於或大於 32 Mb。|
|/bv|新增 **/basevideo**選項設為指定<OSEntryLineNum>，將導向作業系統使用標準的 VGA 模式來安裝的視訊驅動程式。|
|/so|新增 **/sos**選項設為指定*OSEntryLineNum*，導向至顯示裝置驅動程式名稱，而正在載入作業系統。|
|/ng|新增 **/noguiboot**選項設為指定<OSEntryLineNum>，停用出現之前 CTRL + ALT + del 登入提示的進度列。|
|/id <OSEntryLineNum>|Boot.ini 檔案加入作業系統載入選項的 [operating systems] 區段中指定的作業系統項目行號。 [Operating systems] 區段標頭之後的第一行會是 1。|
|/?|在命令提示字元顯示說明。|
## <a name="BKMK_examples"></a>範例
下列範例示範如何使用**bootcfg /addsw**命令：
```
bootcfg /addsw /mm 64 /id 2 
bootcfg /addsw /so /id 3 
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /addsw /ng /id 2 
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)

---
title: bootcfg query
description: 適用於 Windows 命令主題**bootcfg 查詢**-查詢並顯示 [開機載入器] 和 [作業系統] 區段 Boot.ini 中的項目。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a6886fbdfe541b39e530d2b7f71d4fc40909a2a2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862929"
---
# <a name="bootcfg-query"></a>bootcfg query

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

查詢，並顯示 [開機載入器] 和 [operating systems] 區段 Boot.ini 中的項目。

## <a name="syntax"></a>語法
```
bootcfg /query [/s <computer> [/u <Domain>\<User> /p <Password>]]
```
## <a name="parameters"></a>參數
|詞彙|定義|
|----|-------|
|/s <computer>|指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。|
|/u <Domain>\\<User>|使用指定的使用者帳戶權限執行命令<User>或是<Domain> \\ <User>。 預設值是目前登入的使用者發出命令的電腦上的權限。|
|/p <Password>|指定在指定的使用者帳戶的密碼 **/u**參數。|
|/?|在命令提示字元顯示說明。|
##### <a name="remarks"></a>備註
-   以下是範例**bootcfg /query**輸出：
    ```
    Boot Loader Settings
    ----------
    timeout: 30
    default: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
    Boot Entries
    ------
    Boot entry ID:   1
    Friendly Name:   ""
    path:            multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
    OS Load Options: /fastdetect /debug /debugport=com1:
    ```
-   開機載入器設定部分**bootcfg 查詢**輸出會顯示在 Boot.ini 的 [開機載入器] 區段中的每個項目。
-   開機項目部分**bootcfg 查詢**輸出會顯示下列詳細資料的每個作業系統項目 Boot.ini 的 [operating systems] 區段中：開機項目 ID 好記的名稱、 路徑和 OS 載入選項。
## <a name="BKMK_examples"></a>範例
下列範例示範如何使用**bootcfg /query**命令：
```
bootcfg /query
bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
bootcfg /query /u hiropln /p p@ssW23
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)

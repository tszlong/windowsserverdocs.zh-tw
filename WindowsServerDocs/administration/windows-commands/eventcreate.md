---
title: eventcreate
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2b1b26d-a70e-49a6-832b-91eb5a1a159a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80d575364adbeba9d9ea4da75a0a866bcc02acea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818709"
---
# <a name="eventcreate"></a>eventcreate



可讓系統管理員指定的事件記錄檔中建立自訂的事件。 如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
eventcreate [/s <Computer> [/u <Domain\User> [/p <Password>]] {[/l {APPLICATION|SYSTEM}]|[/so <SrcName>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <EventID> /d <Description>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/s\<電腦 >|指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。|
|/u \<Domain\User>|使用指定的使用者帳戶權限執行命令\<使用者 > 或 < 網域 \ 使用者 >。 預設值是目前登入的使用者發出命令的電腦上的權限。|
|/p \<Password>|指定在指定的使用者帳戶的密碼 **/u**參數。|
|/l {APPLICATION\|SYSTEM}|指定將會在當中建立事件的事件記錄檔的名稱。 有效的記錄檔名稱是應用程式和系統。|
|/so \<SrcName>|指定要用於事件的來源。 有效的來源可以是任何字串，並應該代表應用程式或元件產生的事件。|
|/t {錯誤\|警告\|資訊\|</br>SUCCESSAUDIT\|FAILUREAUDIT}|指定要建立事件的類型。 有效的類型為錯誤、 警告、 資訊、 SUCCESSAUDIT 和 FAILUREAUDIT。|
|/id \<EventID >|指定事件的事件識別碼。 有效的識別碼是從 1 到 1000 之間的任何數字。|
|/d\<描述 >|指定要用於新建立的事件描述。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   無法寫入安全性記錄檔的自訂事件。

## <a name="BKMK_examples"></a>範例

下列範例顯示如何使用 eventcreate 命令：
```
eventcreate /t error /id 100 /l application /d "Create event in application log"
eventcreate /t information /id 1000 /so winmgmt /d "Create event in WinMgmt source"
eventcreate /t error /id 2001 /so winword /l application /d "new src Winword in application log"
eventcreate /s server /t error /id 100 /l application /d "Remote machine without user credentials"
eventcreate /s server /u user /p password /id 100 /t error /l application /d "Remote machine with user credentials"
eventcreate /s server1 /s server2 /u user /p password /id 100 /t error /so winmgmt /d "Creating events on Multiple remote machines"
eventcreate /s server /u user /id 100 /t warning /so winmgmt /d "Remote machine with partial user credentials"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

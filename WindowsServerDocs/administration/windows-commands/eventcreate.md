---
title: eventcreate
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f2b1b26d-a70e-49a6-832b-91eb5a1a159a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 797298622ba1021caef3d04e2f2f06f016ef6a70
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725765"
---
# <a name="eventcreate"></a>eventcreate



可讓系統管理員在指定的事件記錄檔中建立自訂事件。 

## <a name="syntax"></a>語法

```
eventcreate [/s <Computer> [/u <Domain\User> [/p <Password>]] {[/l {APPLICATION|SYSTEM}]|[/so <SrcName>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <EventID> /d <Description>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/s \<Computer>|指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。|
|/u \<Domain\User>|使用使用者> 指定之使用者的帳戶許可權，或 <\<Domain\User> 來執行命令。 預設為發出命令之電腦上目前登入使用者的許可權。|
|/p \<密碼>|指定 **/u**參數中指定之使用者帳戶的密碼。|
|/l {應用\|程式系統}|指定將建立事件的事件記錄檔名稱。 有效的記錄檔名稱為 APPLICATION 和 SYSTEM。|
|/so \<SrcName>|指定事件要使用的來源。 有效的來源可以是任何字串，而且應該代表正在產生事件的應用程式或元件。|
|/t {錯誤\|警告\|資訊\|</br>SUCCESSAUDIT\|FAILUREAUDIT}|指定要建立的事件種類。 有效的類型為 ERROR、WARNING、INFORMATION、SUCCESSAUDIT 和 FAILUREAUDIT。|
|/id \<EventID>|指定事件的事件識別碼。 有效的識別碼是從1到1000的任何數位。|
|/d \<描述>|指定要用於新建立事件的描述。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   無法將自訂事件寫入安全性記錄檔。

## <a name="examples"></a>範例

下列範例會示範如何使用 eventcreate 命令：
```
eventcreate /t error /id 100 /l application /d Create event in application log
eventcreate /t information /id 1000 /so winmgmt /d Create event in WinMgmt source
eventcreate /t error /id 2001 /so winword /l application /d new src Winword in application log
eventcreate /s server /t error /id 100 /l application /d Remote machine without user credentials
eventcreate /s server /u user /p password /id 100 /t error /l application /d Remote machine with user credentials
eventcreate /s server1 /s server2 /u user /p password /id 100 /t error /so winmgmt /d Creating events on Multiple remote machines
eventcreate /s server /u user /id 100 /t warning /so winmgmt /d Remote machine with partial user credentials
```

#### <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

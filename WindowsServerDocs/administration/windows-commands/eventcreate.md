---
title: eventcreate
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: cf53d8d269d0994ddf57eb350982effed5e0e702
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377516"
---
# <a name="eventcreate"></a>eventcreate



可讓系統管理員在指定的事件記錄檔中建立自訂事件。 如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
eventcreate [/s <Computer> [/u <Domain\User> [/p <Password>]] {[/l {APPLICATION|SYSTEM}]|[/so <SrcName>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <EventID> /d <Description>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/s \<Computer >|指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。|
|/u \<Domain \ 使用者 >|以 @no__t 0User > 所指定使用者的帳戶許可權執行命令，或 < 網域 \ 使用者 >。 預設為發出命令之電腦上目前登入使用者的許可權。|
|/p \<密碼 >|指定 **/u**參數中指定之使用者帳戶的密碼。|
|/l {APPLICATION @ no__t-0SYSTEM}|指定將建立事件的事件記錄檔名稱。 有效的記錄檔名稱為 APPLICATION 和 SYSTEM。|
|/so \<SrcName >|指定事件要使用的來源。 有效的來源可以是任何字串，而且應該代表正在產生事件的應用程式或元件。|
|/t {ERROR @ no__t-0WARNING @ no__t-1INFORMATION @ no__t-2</br>SUCCESSAUDIT @ NO__T-0FAILUREAUDIT}|指定要建立的事件種類。 有效的類型為 ERROR、WARNING、INFORMATION、SUCCESSAUDIT 和 FAILUREAUDIT。|
|/id \<EventID >|指定事件的事件識別碼。 有效的識別碼是從1到1000的任何數位。|
|/d \<Description >|指定要用於新建立事件的描述。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   無法將自訂事件寫入安全性記錄檔。

## <a name="BKMK_examples"></a>典型

下列範例會示範如何使用 eventcreate 命令：
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

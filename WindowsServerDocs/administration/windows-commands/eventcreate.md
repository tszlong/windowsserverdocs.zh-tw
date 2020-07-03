---
title: eventcreate
description: Eventcreate 命令的參考文章，可讓系統管理員在指定的事件記錄檔中建立自訂事件。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f2b1b26d-a70e-49a6-832b-91eb5a1a159a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60ed97eeffc8ae2410fdd8f296a0e8348f376652
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925315"
---
# <a name="eventcreate"></a>eventcreate

可讓系統管理員在指定的事件記錄檔中建立自訂事件。

> [!IMPORTANT]
> 無法將自訂事件寫入安全性記錄檔。

## <a name="syntax"></a>語法

```
eventcreate [/s <computer> [/u <domain\user> [/p <password>]] {[/l {APPLICATION|SYSTEM}]|[/so <srcname>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <eventID> /d <description>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- |------------ |
| /s`<computer>` | 指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。 |
| u`<domain\user>` | 以或指定之使用者的帳戶許可權來執行命令 `<user>` `<domain\user>` 。 預設為發出命令之電腦上目前登入使用者的許可權。 |
| /p`<password>` | 指定 **/u**參數中指定之使用者帳戶的密碼。 |
| /l`{APPLICATION | SYSTEM}` | 指定將建立事件的事件記錄檔名稱。 有效的記錄檔名稱為**APPLICATION**或**SYSTEM**。 |
| /so`<srcname>` | 指定事件要使用的來源。 有效的來源可以是任何字串，而且應該代表正在產生事件的應用程式或元件。 |
| 一起`{ERROR | WARNING | INFORMATION | SUCCESSAUDIT | FAILUREAUDIT}` | 指定要建立的事件種類。 有效的類型為**ERROR**、 **WARNING**、 **INFORMATION**、 **SUCCESSAUDIT**和**FAILUREAUDIT**。 |
| /id`<eventID>` | 指定事件的事件識別碼。 有效的識別碼是從1到1000的任何數位。 |
| /d`<description>` | 指定要用於新建立事件的描述。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

下列範例會示範如何使用**eventcreate**命令：

```
eventcreate /t error /id 100 /l application /d Create event in application log
eventcreate /t information /id 1000 /so winmgmt /d Create event in WinMgmt source
eventcreate /t error /id 2001 /so winword /l application /d new src Winword in application log
eventcreate /s server /t error /id 100 /l application /d Remote machine without user credentials
eventcreate /s server /u user /p password /id 100 /t error /l application /d Remote machine with user credentials
eventcreate /s server1 /s server2 /u user /p password /id 100 /t error /so winmgmt /d Creating events on Multiple remote machines
eventcreate /s server /u user /id 100 /t warning /so winmgmt /d Remote machine with partial user credentials
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

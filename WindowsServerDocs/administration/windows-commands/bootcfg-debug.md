---
title: bootcfg debug
description: 適用于 bootcfg debug 命令的參考文章，它會加入或變更指定之作業系統專案的偵錯工具設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 28afa5fb-a236-46e2-b1a4-a3c43a49c437
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da4179d85d4e84918e75fb4c8490e229230412eb
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926322"
---
# <a name="bootcfg-debug"></a>bootcfg debug

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

加入或變更指定之作業系統專案的偵錯工具設定。

>[!NOTE]
> 如果您正嘗試調試埠1394，請改用[bootcfg dbg1394](bootcfg-dbg1394.md)命令。

## <a name="syntax"></a>語法

```
bootcfg /debug {on | off | edit}[/s <computer> [/u <domain>\<user> /p <password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <osentrylinenum>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `{on | off | edit}` | 指定埠偵錯工具的值，包括：<ul><li>**的.** 藉由將/debug 選項加入至指定的，啟用遠端偵錯程式支援 `<osentrylinenum>` 。</li><li>**停止.** 從指定的移除/debug 選項，以停用遠端偵錯程式支援 <osentrylinenum> 。</li><li>**編輯.** 藉由變更與指定之/debug 選項關聯的值，允許變更埠和傳輸速率設定 <osentrylinenum> 。</li></ul> |
| `/s <computer>` | 指定遠端電腦的名稱或 IP 位址（不要使用反斜線）。 預設是本機電腦。 |
| `/u <domain>\<user>`  | 以或指定之使用者的帳戶許可權來執行命令 `<user>` `<domain>\<user>` 。 預設為發出命令之電腦上目前登入使用者的許可權。 |
| `/p <password>` | 指定 **/u**參數中指定之使用者帳戶的密碼。 |
| `/port {COM1 | COM2 | COM3 | COM4}` |  指定要用於進行偵錯工具的 COM 埠。 如果停用調試功能，請勿使用此參數。 |
| `/baud {9600 | 19200 | 38400 | 57600 | 115200}` | 指定用於進行偵錯工具的傳輸速率。 如果停用調試功能，請勿使用此參數。 |
| `/id <osentrylinenum>` | 在新增作業系統載入選項的 Boot.ini 檔案的 [作業系統] 區段中，指定作業系統專案行號。 [作業系統] 區段標頭後面的第一行是1。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要使用**bootcfg/debug**命令：

```
bootcfg /debug on /port com1 /id 2
bootcfg /debug edit /port com2 /baud 19200 /id 2
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bootcfg 命令](bootcfg.md)

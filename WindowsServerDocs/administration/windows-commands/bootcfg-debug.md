---
title: bootcfg debug
description: 適用于 bootcfg debug 命令的參考文章，此命令會針對指定的作業系統專案新增或變更偵錯工具設定。
ms.topic: reference
ms.assetid: 28afa5fb-a236-46e2-b1a4-a3c43a49c437
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 385bdd6ebadb4cb3eb9b48e1b920325db996dc38
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630241"
---
# <a name="bootcfg-debug"></a>bootcfg debug

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

新增或變更指定之作業系統專案的偵錯工具設定。

>[!NOTE]
> 如果您正在嘗試對埠1394進行調試，請改用 [bootcfg dbg1394](bootcfg-dbg1394.md) 命令。

## <a name="syntax"></a>語法

```
bootcfg /debug {on | off | edit}[/s <computer> [/u <domain>\<user> /p <password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <osentrylinenum>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `{on | off | edit}` | 指定埠偵錯工具的值，包括：<ul><li>**個.** 藉由將/debug 選項加入指定的，啟用遠端偵錯程式支援 `<osentrylinenum>` 。</li><li>**偏離.** 從指定的移除/debug 選項，以停用遠端偵錯程式支援 <osentrylinenum> 。</li><li>**編輯。** 允許變更埠和傳輸速率設定，方法是變更指定之/debug 選項的關聯值 <osentrylinenum> 。</li></ul> |
| `/s <computer>` | 指定遠端電腦的名稱或 IP 位址 (不要使用反斜線) 。 預設是本機電腦。 |
| `/u <domain>\<user>`  | 使用或所指定之使用者的帳戶許可權來執行命令 `<user>` `<domain>\<user>` 。 預設值是發出命令的電腦上目前登入之使用者的許可權。 |
| `/p <password>` | 指定 **/u** 參數中指定之使用者帳戶的密碼。 |
| `/port {COM1 | COM2 | COM3 | COM4}` |  指定要用於偵錯工具的 COM 通訊埠。 如果停用偵錯工具，請勿使用此參數。 |
| `/baud {9600 | 19200 | 38400 | 57600 | 115200}` | 指定要用於偵錯工具的傳輸速率。 如果停用偵錯工具，請勿使用此參數。 |
| `/id <osentrylinenum>` | 在新增作業系統載入選項的 Boot.ini 檔案的 [作業系統] 區段中，指定作業系統專案行號。 [作業系統] 區段標頭之後的第一行是1。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要使用 **bootcfg/debug** 命令：

```
bootcfg /debug on /port com1 /id 2
bootcfg /debug edit /port com2 /baud 19200 /id 2
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bootcfg 命令](bootcfg.md)

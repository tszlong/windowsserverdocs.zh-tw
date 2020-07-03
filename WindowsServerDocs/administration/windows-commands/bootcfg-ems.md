---
title: bootcfg ems
description: 適用于 bootcfg ems 命令的參考文章，可讓使用者新增或變更緊急管理服務主控台重新導向至遠端電腦的設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57abdc50-c64a-45f1-8470-3f8c3a51f743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c24f8acf6beb368dd989e4b05c912b69c4e7b68
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926242"
---
# <a name="bootcfg-ems"></a>bootcfg ems

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓使用者新增或變更緊急管理服務主控台重新導向至遠端電腦的設定。 啟用緊急管理服務時，會將 `redirect=Port#` 一行新增至 Boot.ini 檔案的 [開機載入器] 區段，並將/redirect 選項加到指定的作業系統專案行。 [緊急管理服務] 功能只會在伺服器上啟用。

## <a name="syntax"></a>語法

```
bootcfg /ems {on | off | edit}[/s <computer> [/u <domain>\<user> /p <password>]] [/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <osentrylinenum>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `{on | off | edit}` | 指定緊急管理服務重新導向的值，包括：<ul><li>**的.** 啟用所指定的遠端輸出 `<osentrylinenum>` 。 也會將/redirect 選項加入至指定的 <osentrylinenum> ，並將 `redirect=com<X>` 設定新增至 [開機載入器] 區段。 的值 `com<X>` 是由 **/port**參數所設定。</li><li>**停止.** 停用輸出至遠端電腦。 也會 <osentrylinenum> `redirect=com<X>` 從 [開機載入器] 區段中，移除指定之和設定的/redirect 選項。</li><li>**編輯.** 藉由變更 `redirect=com<X>` [開機載入器] 區段中的設定，允許變更埠設定。 的值 `com<X>` 是由 **/port**參數所設定。</li></ul> |
| `/s <computer>` | 指定遠端電腦的名稱或 IP 位址（不要使用反斜線）。 預設是本機電腦。 |
| `/u <domain>\<user>`  | 以或指定之使用者的帳戶許可權來執行命令 `<user>` `<domain>\<user>` 。 預設為發出命令之電腦上目前登入使用者的許可權。 |
| `/p <password>` | 指定 **/u**參數中指定之使用者帳戶的密碼。 |
| `/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}` |  指定要用於重新導向的 COM 埠。 BIOSSET 參數會指示緊急管理服務取得 BIOS 設定，以判斷應該使用哪一個埠進行重新導向。 如果已停用遠端系統管理的輸出，請勿使用此參數。 |
| `/baud {9600 | 19200 | 38400 | 57600 | 115200}` | 指定要用於重新導向的傳輸速率。 如果已停用遠端系統管理的輸出，請勿使用此參數。 |
| `/id <osentrylinenum>` | 指定 [緊急管理服務] 選項在 Boot.ini 檔案的 [作業系統] 區段中新增的作業系統專案行號。 [作業系統] 區段標頭後面的第一行是1。 當 [緊急管理服務] 值設定為 [**開啟**] 或 [**關閉**] 時，此為必要參數。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要使用**bootcfg/ems**命令：

```
bootcfg /ems on /port com1 /baud 19200 /id 2
bootcfg /ems on /port biosset /id 3
bootcfg /s srvmain /ems off /id 2
bootcfg /ems edit /port com2 /baud 115200
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /ems off /id 2
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bootcfg 命令](bootcfg.md)

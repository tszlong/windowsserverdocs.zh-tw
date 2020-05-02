---
title: bootcfg ems
description: Bootcfg ems 命令的參考主題，可讓使用者新增或變更緊急管理服務主控台重新導向至遠端電腦的設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57abdc50-c64a-45f1-8470-3f8c3a51f743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c85231e094852feb673eb4f99b183f06014234b2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709526"
---
# <a name="bootcfg-ems"></a>bootcfg ems

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓使用者新增或變更緊急管理服務主控台重新導向至遠端電腦的設定。 啟用緊急管理服務時，會`redirect=Port#`在 boot.ini 檔案的 [開機載入器] 區段中加上一行，並在指定的作業系統專案行中加上/redirect 選項。 [緊急管理服務] 功能只會在伺服器上啟用。

## <a name="syntax"></a>語法

```
bootcfg /ems {on | off | edit}[/s <computer> [/u <domain>\<user> /p <password>]] [/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <osentrylinenum>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `{on | off | edit}` | 指定緊急管理服務重新導向的值，包括：<ul><li>**的.** 啟用所指定`<osentrylinenum>`的遠端輸出。 也會將/redirect 選項加入至指定<osentrylinenum>的， `redirect=com<X>`並將設定新增至 [開機載入器] 區段。 的值`com<X>`是由 **/port**參數所設定。</li><li>**停止.** 停用輸出至遠端電腦。 也會從 [開機載入器] <osentrylinenum>區段中`redirect=com<X>` ，移除指定之和設定的/redirect 選項。</li><li>**編輯.** 藉由變更 [開機載入器] `redirect=com<X>`區段中的設定，允許變更埠設定。 的值`com<X>`是由 **/port**參數所設定。</li></ul> |
| `/s <computer>` | 指定遠端電腦的名稱或 IP 位址（不要使用反斜線）。 預設是本機電腦。 |
| `/u <domain>\<user>`  | 以`<user>`或`<domain>\<user>`指定之使用者的帳戶許可權來執行命令。 預設為發出命令之電腦上目前登入使用者的許可權。 |
| `/p <password>` | 指定 **/u**參數中指定之使用者帳戶的密碼。 |
| `/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}` |  指定要用於重新導向的 COM 埠。 BIOSSET 參數會指示緊急管理服務取得 BIOS 設定，以判斷應該使用哪一個埠進行重新導向。 如果已停用遠端系統管理的輸出，請勿使用此參數。 |
| `/baud {9600 | 19200 | 38400 | 57600 | 115200}` | 指定要用於重新導向的傳輸速率。 如果已停用遠端系統管理的輸出，請勿使用此參數。 |
| `/id <osentrylinenum>` | 指定在 Boot.ini 檔案的 [作業系統] 區段中新增 [緊急管理服務] 選項的作業系統專案行號。 [作業系統] 區段標頭後面的第一行是1。 當 [緊急管理服務] 值設定為 [**開啟**] 或 [**關閉**] 時，此為必要參數。 |
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

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bootcfg 命令](bootcfg.md)

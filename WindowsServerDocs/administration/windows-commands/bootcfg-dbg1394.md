---
title: bootcfg dbg1394
description: Bootcfg dbg1394 命令的參考文章，其會為指定的作業系統專案設定1394埠的偵錯工具
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 35724697-90dd-4dbe-85b0-337fbd369dcc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1f71edbabbf85c301bec24138a805523975d3f6
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926340"
---
# <a name="bootcfg-dbg1394"></a>bootcfg dbg1394

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

為指定的作業系統專案設定1394埠的偵錯工具。

## <a name="syntax"></a>語法

```
bootcfg /dbg1394 {on | off}[/s <computer> [/u <domain>\<user> /p <password>]] [/ch <channel>] /id <osentrylinenum>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `{on | off}` | 指定1394埠偵錯工具的值，包括：<ul><li>**的.** 藉由將/dbg1394 選項加入至指定的，啟用遠端偵錯程式支援 `<osentrylinenum>` 。</li><li>**停止.** 從指定的移除/dbg1394 選項，以停用遠端偵錯程式支援 <osentrylinenum> 。</li></ul> |
| `/s <computer>` | 指定遠端電腦的名稱或 IP 位址（不要使用反斜線）。 預設是本機電腦。 |
| `/u <domain>\<user>`  | 以或指定之使用者的帳戶許可權來執行命令 `<user>` `<domain>\<user>` 。 預設為發出命令之電腦上目前登入使用者的許可權。 |
| `/p <password>` | 指定 **/u**參數中指定之使用者帳戶的密碼。 |
| `/ch <channel>` | 指定用於進行偵錯工具的通道。 有效的值包括整數，介於1到64之間。 如果停用1394埠的調試功能，請勿使用此參數。 |
| `/id <osentrylinenum>` | 在新增作業系統載入選項的 Boot.ini 檔案的 [作業系統] 區段中，指定作業系統專案行號。 [作業系統] 區段標頭後面的第一行是1。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要使用**bootcfg/dbg1394**命令：

```
bootcfg /dbg1394 /id 2
bootcfg /dbg1394 on /ch 1 /id 3
bootcfg /dbg1394 edit /ch 8 /id 2
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /dbg1394 off /id 2
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bootcfg 命令](bootcfg.md)

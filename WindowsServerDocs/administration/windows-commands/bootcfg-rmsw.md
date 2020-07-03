---
title: bootcfg rmsw
description: Bootcfg rmsw 命令的參考文章，它會移除指定作業系統專案的作業系統載入選項。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fd7e4248-880e-4e2b-929e-87f8d44b9a63
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c905712b898501f45cbfc036d771f18232e82d5b
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924975"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

移除指定之作業系統專案的作業系統載入選項。

## <a name="syntax"></a>語法

```
bootcfg /rmsw [/s <computer> [/u <domain>\<user> /p <password>]] [/mm] [/bv] [/so] [/ng] /id <osentrylinenum>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `/s <computer>` | 指定遠端電腦的名稱或 IP 位址（不要使用反斜線）。 預設是本機電腦。 |
| `/u <domain>\<user>`  | 以或指定之使用者的帳戶許可權來執行命令 `<user>` `<domain>\<user>` 。 預設為發出命令之電腦上目前登入使用者的許可權。 |
| `/p <password>` | 指定 **/u**參數中指定之使用者帳戶的密碼。 |
| /mm | 從指定的中移除/maxmem 選項及其關聯的最大記憶體值 `<osentrylinenum>` 。 /Maxmem 選項會指定作業系統可使用的 RAM 數量上限。 |
| /bv | 從指定的移除/basevideo 選項 `<osentrylinenum>` 。 /Basevideo 選項會指示作業系統使用標準 VGA 模式來安裝視頻驅動程式。 |
| /so | 從指定的移除/sos 選項 `<osentrylinenum>` 。 /Sos 選項會指示作業系統在載入時顯示裝置磁碟機名稱。 |
| /ng | 從指定的移除/noguiboot 選項 `<osentrylinenum>` 。 /Noguiboot 選項會停用在 CTRL + ALT + DEL 登入提示字元之前出現的進度列。 |
| `/id <osentrylinenum>` | 在新增作業系統載入選項的 Boot.ini 檔案的 [作業系統] 區段中，指定作業系統專案行號。 [作業系統] 區段標頭後面的第一行是1。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要使用**bootcfg/rmsw**命令：

```
bootcfg /rmsw /mm 64 /id 2
bootcfg /rmsw /so /id 3
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2
bootcfg /rmsw /ng /id 2
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bootcfg 命令](bootcfg.md)

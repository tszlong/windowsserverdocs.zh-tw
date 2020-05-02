---
title: bootcfg raw
description: Bootcfg raw 命令的參考主題，其會將作業系統載入選項（指定為字串）新增至 Boot.ini 檔案之作業系統區段中的作業系統專案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3458749-b0a0-460f-a022-3ff199a71f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9127b278a41bb8f1f36f7b45c544bf09f5c4ff7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82708948"
---
# <a name="bootcfg-raw"></a>bootcfg raw

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將指定為字串的作業系統載入選項新增至 Boot.ini 檔案的 [作業系統] 區段中的作業系統專案。 此命令會覆寫任何現有的作業系統專案選項。

## <a name="syntax"></a>語法

```
bootcfg /raw [/s <computer> [/u <domain>\<user> /p <password>]] <osloadoptionsstring> [/id <osentrylinenum>] [/a]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `/s <computer>` | 指定遠端電腦的名稱或 IP 位址（不要使用反斜線）。 預設是本機電腦。 |
| `/u <domain>\<user>`  | 以`<user>`或`<domain>\<user>`指定之使用者的帳戶許可權來執行命令。 預設為發出命令之電腦上目前登入使用者的許可權。 |
| `/p <password>` | 指定 **/u**參數中指定之使用者帳戶的密碼。 |
| `<osloadoptionsstring>` | 指定要新增至作業系統專案的作業系統載入選項。 這些載入選項會取代任何與作業系統專案相關聯的現有載入選項。 不會對`<osloadoptions>`參數進行任何驗證。
| `/id <osentrylinenum>` | 在新增作業系統載入選項的 Boot.ini 檔案的 [作業系統] 區段中，指定作業系統專案行號。 [作業系統] 區段標頭後面的第一行是1。 |
| /a | 指定應該將哪些作業系統選項附加至任何現有的作業系統選項。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

此文字應包含有效的 OS 載入選項，例如 **/debug**、 **/fastdetect**、 **/nodebug**、 **/baudrate**、 **/crashdebug**和 **/sos**。

若要將 **/debug/fastdetect**新增至第一個作業系統專案的結尾，請取代任何先前的作業系統專案選項：

```
bootcfg /raw /debug /fastdetect /id 1
```

若要使用**bootcfg/raw**命令：

```
bootcfg /raw /debug /sos /id 2
bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 /crashdebug  /id 2
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bootcfg 命令](bootcfg.md)

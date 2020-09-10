---
title: bootcfg raw
description: 適用于 bootcfg raw 命令的參考文章，此命令會將作業系統載入選項（指定為字串）新增至 Boot.ini 檔之作業系統區段中的作業系統專案。
ms.topic: reference
ms.assetid: e3458749-b0a0-460f-a022-3ff199a71f27
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b86945a126c73742982ea01442101c6d1250226d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630139"
---
# <a name="bootcfg-raw"></a>bootcfg raw

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Boot.ini 檔案的 [作業系統] 區段中，將指定為字串的作業系統載入選項新增至作業系統專案。 此命令會覆寫任何現有的作業系統專案選項。

## <a name="syntax"></a>語法

```
bootcfg /raw [/s <computer> [/u <domain>\<user> /p <password>]] <osloadoptionsstring> [/id <osentrylinenum>] [/a]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `/s <computer>` | 指定遠端電腦的名稱或 IP 位址 (不要使用反斜線) 。 預設是本機電腦。 |
| `/u <domain>\<user>`  | 使用或所指定之使用者的帳戶許可權來執行命令 `<user>` `<domain>\<user>` 。 預設值是發出命令的電腦上目前登入之使用者的許可權。 |
| `/p <password>` | 指定 **/u** 參數中指定之使用者帳戶的密碼。 |
| `<osloadoptionsstring>` | 指定要新增至作業系統專案的作業系統載入選項。 這些載入選項會取代任何與作業系統專案相關聯的現有載入選項。 沒有針對參數的驗證 `<osloadoptions>` 。
| `/id <osentrylinenum>` | 在新增作業系統載入選項的 Boot.ini 檔案的 [作業系統] 區段中，指定作業系統專案行號。 [作業系統] 區段標頭之後的第一行是1。 |
| /a | 指定應將哪些作業系統選項附加至任何現有的作業系統選項。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

此文字應包含有效的 OS 載入選項，例如 **/debug**、 **/fastdetect**、 **/nodebug**、 **/baudrate**、 **/crashdebug**和 **/sos**。

若要將 **/debug/fastdetect** 新增至第一個作業系統專案的結尾，請取代任何先前的作業系統專案選項：

```
bootcfg /raw /debug /fastdetect /id 1
```

若要使用 **bootcfg/raw** 命令：

```
bootcfg /raw /debug /sos /id 2
bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 /crashdebug  /id 2
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bootcfg 命令](bootcfg.md)

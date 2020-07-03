---
title: bootcfg query
description: Bootcfg 查詢命令的參考文章，它會查詢並顯示 Boot.ini 的開機載入器和作業系統區段專案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8c99ba9bfbe910c65b3686c6070a7a76f27407f5
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925011"
---
# <a name="bootcfg-query"></a>bootcfg query

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

查詢並顯示 Boot.ini 的 [開機載入器] 和 [作業系統] 區段專案。

## <a name="syntax"></a>語法

```
bootcfg /query [/s <computer> [/u <domain>\<user> /p <password>]]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `/s <computer>` | 指定遠端電腦的名稱或 IP 位址（不要使用反斜線）。 預設是本機電腦。 |
| `/u <domain>\<user>`  | 以或指定之使用者的帳戶許可權來執行命令 `<user>` `<domain>\<user>` 。 預設為發出命令之電腦上目前登入使用者的許可權。 |
| `/p <password>` | 指定 **/u**參數中指定之使用者帳戶的密碼。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="sample-output"></a>範例輸出

**Bootcfg/query**命令的範例輸出：

```
Boot Loader Settings
----------
timeout: 30
default: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
Boot Entries
------
Boot entry ID:   1
Friendly Name:
path: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
OS Load Options: /fastdetect /debug /debugport=com1:
```

- [**開機載入器設定**] 區域會顯示 Boot.ini [開機載入器] 區段中的每個專案。

- [**開機專案**] 區域會針對 [作業系統] 區段中的每個作業系統專案顯示更多詳細資料 Boot.ini

## <a name="examples"></a>範例

若要使用**bootcfg/query**命令：

```
bootcfg /query
bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
bootcfg /query /u hiropln /p p@ssW23
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bootcfg 命令](bootcfg.md)

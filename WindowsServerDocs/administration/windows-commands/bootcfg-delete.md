---
title: bootcfg delete
description: Bootcfg delete 命令的參考文章，此命令會在 Boot.ini 檔案的作業系統區段中刪除作業系統專案。
ms.topic: reference
ms.assetid: 71382e29-9b39-41c8-9c23-cf0ff829440a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9eedad8157dbf25167c484e276167c038adb5667
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034466"
---
# <a name="bootcfg-delete"></a>bootcfg delete

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Boot.ini 檔案的 [作業系統] 區段中刪除作業系統專案。

## <a name="syntax"></a>語法

```
bootcfg /delete [/s <computer> [/u <domain>\<user> /p <password>]] [/id <osentrylinenum>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `/s <computer>` | 指定遠端電腦的名稱或 IP 位址 (不要使用反斜線) 。 預設是本機電腦。 |
| `/u <domain>\<user>`  | 使用或所指定之使用者的帳戶許可權來執行命令 `<user>` `<domain>\<user>` 。 預設值是發出命令的電腦上目前登入之使用者的許可權。 |
| `/p <password>` | 指定 **/u** 參數中指定之使用者帳戶的密碼。 |
| `/id <osentrylinenum>` | 在新增作業系統載入選項的 Boot.ini 檔案的 [作業系統] 區段中，指定作業系統專案行號。 [作業系統] 區段標頭之後的第一行是1。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要使用 **bootcfg/delete** 命令：

```
bootcfg /delete /id 1
bootcfg /delete /s srvmain /u maindom\hiropln /p p@ssW23 /id 3
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bootcfg 命令](bootcfg.md)

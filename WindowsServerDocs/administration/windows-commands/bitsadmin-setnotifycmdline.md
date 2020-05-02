---
title: bitsadmin setnotifycmdline
description: Bitsadmin setnotifycmdline 命令的參考主題，它會設定當作業完成傳輸資料或工作進入狀態時，將會執行的命令列命令。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b21d7151a5b646a4fe07d073220614f5e3c99539
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720127"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

設定在作業完成傳輸資料或工作進入指定狀態之後執行的命令列命令。

> [!NOTE]
> BITS 1.2 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /setnotifycmdline <job> <program_name> [program_parameters]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| program_name | 作業完成時要執行的命令名稱。 您可以將此值設定為 Null，但如果您這樣做， *program_parameters*也必須設定為 null。 |
| program_parameters | 您想要傳遞給*program_name*的參數。 您可以將此值設定為 Null。 如果*program_parameters*未設定為 Null，則*program_parameters*中的第一個參數必須符合*program_name*。 |

## <a name="examples"></a>範例

若要在名為*myDownloadJob*的工作完成時執行 notepad.exe：

```
bitsadmin /setnotifycmdline myDownloadJob c:\winnt\system32\notepad.exe NULL
```

若要在 Notepad.exe 中顯示 EULA 文字，請在作業完成時，將其命名為 myDownloadJob：

```
bitsadmin /setnotifycmdline myDownloadJob c:\winnt\system32\notepad.exe notepad c:\eula.txt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

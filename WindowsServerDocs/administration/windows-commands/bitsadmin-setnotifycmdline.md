---
title: bitsadmin setnotifycmdline
description: 適用于**bitsadmin setnotifycmdline**的 Windows 命令主題，它會設定當作業完成傳輸資料或工作進入狀態時，將執行的命令列命令。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b268b68cbd355a7fe7f993d678a98f6fcb99f0ab
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122890"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

設定當作業完成傳輸資料或工作進入指定狀態時，將會執行的命令列命令。

> [!NOTE]
> BITS 1.2 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /setnotifycmdline <job> <program_name> [program_parameters]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| program_name | 作業完成時要執行的命令名稱。 您可以將此值設定為 Null，但如果您這樣做， *program_parameters*也必須設定為 null。 |
| program_parameters | 您想要傳遞給*program_name*的參數。 您可以將此值設定為 Null。 如果*program_parameters*未設定為 Null，則*program_parameters*中的第一個參數必須符合*program_name*。 |

## <a name="examples"></a>範例

下列範例會在名為*myDownloadJob*的作業完成之後，設定服務用來執行 notepad.exe 的命令列命令。

```
C:\>bitsadmin /setnotifycmdline myDownloadJob c:\winnt\system32\notepad.exe NULL
```

```
C:\>bitsadmin /setnotifycmdline myDownloadJob c:\winnt\system32\notepad.exe notepad c:\eula.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
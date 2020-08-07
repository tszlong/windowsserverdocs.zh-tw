---
title: type
description: 類型的參考文章，可顯示文字檔的內容。
ms.topic: article
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 1cb4e28a079c39aaadd85267c004781cc9c84f3c
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896660"
---
# <a name="type"></a>type

在 Windows 命令 shell 中，**輸入**是一個內建的命令，它會顯示文字檔的內容。 使用 [**類型**] 命令來查看文字檔，而不進行修改。

在 PowerShell 中，**輸入**為**[Get Content](/powershell/module/microsoft.powershell.management/get-content)** Cmdlet 的內建別名，這也會顯示檔案的內容，但使用不同的語法。

## <a name="syntax"></a>語法

```
type [<Drive>:][<Path>]<FileName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[\<Drive>:][\<Path>]\<FileName>|指定您想要查看之檔案的位置和名稱。 以空格分隔多個檔案名。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   如果*FileName*包含空格，請將它括在引號中 (例如，包含 Spaces.txt) 的檔案名。
-   如果您顯示二進位檔案或程式所建立的檔案，您可能會在畫面上看到奇怪的字元，包括換頁字元字元和 escape 序列符號。 這些字元代表二進位檔案中所使用的控制項程式碼。 一般來說，請避免使用**type**命令來顯示二進位檔案。

## <a name="examples"></a>範例

若要顯示名為節日的檔案內容，請輸入：
```
type holiday.mar
```
以顯示名為假日的冗長檔案內容。一次只需 mar 一個畫面，請輸入：
```
type holiday.mar | more
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

---
title: 類型
description: Type 命令的參考文章，會顯示文字檔的內容。
ms.topic: reference
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.openlocfilehash: 879cd68b4995c65d623d56e97c0680587eda15b4
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156360"
---
# <a name="type"></a>類型

在 Windows 命令列介面中， **類型** 是內建命令，可顯示文字檔的內容。 使用 **type** 命令來查看文字檔，而不加以修改。

在 PowerShell 中， **類型** 是 [Get Content Cmdlet](/powershell/module/microsoft.powershell.management/get-content)的內建別名，它也會顯示檔案的內容，但使用不同的語法。

## <a name="syntax"></a>語法

```
type [<drive>:][<path>]<filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `[<drive>:][<path>]<filename>` | 指定您想要查看之檔案的位置和名稱。 如果您 `<filename>` 包含空格，您必須將它括在引號中 (例如「包含 Spaces.txt 的檔案名」 ) 。 您也可以加入多個檔案名，方法是在兩者之間加上空格。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您顯示二進位檔案或程式所建立的檔案，您可能會在螢幕上看到奇怪的字元，包括換頁字元字元和 escape 序列符號。 這些字元代表二進位檔案中使用的控制項碼。 一般來說，請避免使用 **type** 命令來顯示二進位檔案。

## <a name="examples"></a>範例

若要顯示名為 *假日*的檔案內容，請輸入：

```
type holiday.mar
```

若要顯示名為 *假日* 的冗長檔案的內容，請在一段時間內顯示一個畫面，輸入：

```
type holiday.mar | more
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

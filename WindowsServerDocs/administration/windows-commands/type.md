---
title: 型別
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 4ceb7365d34a2aeca21d1a699730a589f98fd549
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887399"
---
# <a name="type"></a>型別


在 Windows 命令殼層中，**型別**是內建命令，以顯示文字檔的內容。 使用**型別**命令來檢視文字檔案，而不需要修改它。


在 PowerShell 中，**型別**是內建的別名，以**[Get-content](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-content)** cmdlet 也會顯示的內容檔案，但有不同的語法。


如需如何使用此命令在 Windows 命令殼層 (Cmd.exe) 中的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
type [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[\<Drive>:][\<Path>]\<FileName>|指定的檔案或您想要檢視的檔案名稱與位置。 以空格分隔多個檔案名稱。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   如果*FileName*包含空格，將它括在引號內 (例如，「 檔案名稱包含 Spaces.txt")。
-   如果您顯示二進位檔或程式所建立的檔案，您可能會在畫面上，包括換頁字元的字元和逸出序列符號看到奇怪的字元。 這些字元代表二進位檔中所使用的控制碼。 一般情況下，請避免使用**型別**命令，以顯示二進位檔。

## <a name="BKMK_examples"></a>範例

若要顯示名為 Holiday.mar 檔案的內容，請輸入：
```
type holiday.mar 
```
若要顯示的冗長的檔案，名為 Holiday.mar 一個畫面，一次內容，請輸入：
```
type holiday.mar | more 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

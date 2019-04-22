---
title: compact
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 429b3752-df0a-43a4-a210-df2f3ad03c3b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 068111b293a3eb3987b14744a1bfcf2fde26bced
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826029"
---
# <a name="compact"></a>compact



顯示或變更檔案或目錄的 NTFS 磁碟分割的壓縮。 如果未指定參數，使用**compact**會顯示目前的目錄和其中所含的檔案的壓縮狀態。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
compact [/c | /u] [/s[:<Dir>]] [/a] [/i] [/f] [/q] [<FileName>[...]]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/c|將指定的目錄或檔案的壓縮。|
|/u|指定的目錄或檔案解壓縮。|
|/s[:\<Dir>]|適用於**compact**命令指定的目錄 （或如果未指定目前的目錄） 的所有子目錄。|
|/a|顯示隱藏或系統檔案。|
|/i|忽略錯誤。|
|/f|強制壓縮或解壓縮的指定的目錄或檔案。 **/f**作業被中斷系統損壞時，部分已壓縮的檔案，將使用。 若要強制壓縮整個檔案，請使用 **/c**並 **/f**參數並指定壓縮的檔案的一部份。|
|/q|報告只會將最重要的資訊。|
|\<FileName>|指定的檔案或目錄。 您可以使用多個檔案名稱，而 **&#42;** 並**嗎？** 萬用字元。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   **Compact**命令是 NTFS 檔案系統壓縮功能的命令列版本。 目錄的壓縮狀態表示新增至目錄時，是否會自動壓縮檔案。 設定目錄的壓縮狀態，不一定是變更已在目錄中檔案的壓縮狀態。
-   您無法使用**compact**來讀取、 寫入或使用壓縮或時已壓縮的掛接磁碟區。
-   您無法使用**compact**壓縮檔案配置表 (FAT) 或 FAT32 磁碟分割。

## <a name="BKMK_examples"></a>範例

若要設定目前的目錄、 子目錄和現有檔案的壓縮狀態，請輸入：
```
compact /c /s 
```
若要設定目前的目錄中檔案和子目錄的壓縮狀態，而不需要變更目前的目錄本身的壓縮狀態，請輸入：
```
compact /c /s *.*
```
若要壓縮的磁碟區，從磁碟區的根目錄中，輸入：
```
compact /c /i /s:\
```

> [!NOTE]
> 此範例設定 （包括磁碟區上的根目錄） 的所有目錄的壓縮狀態，並將壓縮的磁碟區上的每個檔案。 **/I**參數可防止錯誤訊息中斷壓縮程序。

若要壓縮.bmp 副檔名 \Tmp 目錄中的所有檔案和 \Tmp，所有子目錄，而不需修改壓縮的目錄的屬性，請輸入：
```
compact /c /s:\tmp *.bmp
```
若要強制 Zebra.bmp 部分過程中發現系統損壞時已壓縮，檔案的完整壓縮輸入：
```
compact /c /f zebra.bmp
```
若要壓縮的屬性從目錄中移除 C:\Tmp，而不需要變更該目錄中的任何檔案的壓縮狀態，請輸入：
```
compact /u c:\tmp
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
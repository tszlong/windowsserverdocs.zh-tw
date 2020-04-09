---
title: 壓縮
description: Compact 的 Windows 命令主題，它會顯示或變更 NTFS 磁碟分割上的檔案或目錄壓縮。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 429b3752-df0a-43a4-a210-df2f3ad03c3b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e9e6a3ba71ecc0c8e264ac4af8dc1da42d23fdc2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847351"
---
# <a name="compact"></a>壓縮

顯示或變更 NTFS 磁碟分割上的檔案或目錄壓縮。 如果使用時不含參數， **compact**會顯示目前目錄及其內含檔案的壓縮狀態。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
compact [/c | /u] [/s[:<Dir>]] [/a] [/i] [/f] [/q] [<FileName>[...]]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/c|壓縮指定的目錄或檔案。|
|/u|Uncompresses 指定的目錄或檔案。|
|/s [：\<Dir >]|將**compact**命令套用至指定目錄（如果未指定，則為目前的目錄）的所有子目錄。|
|/a|顯示隱藏或系統檔案。|
|/i|忽略錯誤。|
|/f|強制壓縮或解壓縮指定的目錄或檔案。 如果檔案在系統損毀中斷時被部分壓縮，則會使用 **/f** 。 若要強制完整壓縮檔案，請使用 **/c**和 **/f**參數，並指定部分壓縮檔案。|
|/q|只報告最重要的資訊。|
|\<FileName >|指定檔案或目錄。 您可以使用多個檔案名和 **&#42;** 和 **？** 萬用字元。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   **Compact**命令是 NTFS 檔案系統壓縮功能的命令列版本。 目錄的壓縮狀態指出當檔案新增至目錄時，是否會自動壓縮檔案。 設定目錄的壓縮狀態不一定會變更目錄中已存在之檔案的壓縮狀態。
-   您無法使用**compact**來讀取、寫入或掛接已使用 [磁碟空間] 或 [DoubleSpace] 壓縮的磁片區。
-   您不能使用**compact**來壓縮檔案分配表（FAT）或 FAT32 磁碟分割。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要設定目前目錄、其子目錄和現有檔案的壓縮狀態，請輸入：
```
compact /c /s 
```
若要設定目前目錄中檔案和子目錄的壓縮狀態，而不改變目前目錄本身的壓縮狀態，請輸入：
```
compact /c /s *.*
```
若要壓縮磁片區，請在磁片區的根目錄中輸入：
```
compact /c /i /s:\
```

> [!NOTE]
> 這個範例會設定所有目錄（包括磁片區上的根目錄）的壓縮狀態，並壓縮磁片區上的每個檔案。 **/I**參數可防止錯誤訊息中斷壓縮進程。

若要使用 \Tmp 目錄中的 .bmp 副檔名和 \Tmp 的所有子目錄來壓縮所有檔案，而不修改目錄的壓縮屬性，請輸入：
```
compact /c /s:\tmp *.bmp
```
若要強制執行在系統損毀期間部分壓縮的檔案 Zebra 的完整壓縮，請輸入：
```
compact /c /f zebra.bmp
```
若要從目錄 C:\Tmp 中移除壓縮的屬性，而不變更該目錄中任何檔案的壓縮狀態，請輸入：
```
compact /u c:\tmp
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
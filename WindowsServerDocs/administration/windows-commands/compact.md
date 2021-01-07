---
title: compact
description: Compact 命令的參考文章，可顯示或改變 NTFS 磁碟分割上的檔案或目錄壓縮。
ms.topic: reference
ms.assetid: 429b3752-df0a-43a4-a210-df2f3ad03c3b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 12c835071c06255a638e65f0bdb78b0d8816cb4d
ms.sourcegitcommit: 528bdff90a7c797cdfc6839e5586f2cd5f0506b0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97977243"
---
# <a name="compact"></a>compact

顯示或改變 NTFS 磁碟分割上的檔案或目錄壓縮。 如果使用時不含參數， **compact** 會顯示目前目錄及其包含之任何檔案的壓縮狀態。

## <a name="syntax"></a>語法

```
compact [/C | /U] [/S[:dir]] [/A] [/I] [/F] [/Q] [/EXE[:algorithm]] [/CompactOs[:option] [/windir:dir]] [filename [...]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /c | 壓縮指定的目錄或檔案。 除非指定了/EXE 參數，否則會將目錄標示為，之後新增的任何檔案都會壓縮。 |
| /U | Uncompresses 指定的目錄或檔案。 目錄會標示為，之後新增的任何檔案都不會壓縮。 如果指定/EXE 參數，則只有壓縮為可執行檔的檔案會解壓縮;如果您未指定/EXE 參數，則只會壓縮 NTFS 壓縮檔案。 |
| /s`[:<dir>]` | 在指定目錄和所有子目錄中的檔案上執行所選作業。 預設會使用目前的目錄做為 `<dir>` 值。 |
| /a | 顯示隱藏的或系統檔案。 依預設，不會包含這些檔案。 |
| /i | 繼續執行指定的作業，並忽略錯誤。 根據預設，此命令會在遇到錯誤時停止。 |
| /f | 強制壓縮或解壓縮指定的目錄或檔案。 預設會略過壓縮檔案。 當系統損毀中斷作業時，會使用 **/f** 參數做為部分壓縮檔案的大小寫。 若要強制壓縮檔案，請使用 **/c** 和 **/f** 參數，並指定部分壓縮檔案。 |
| /q | 只報告最重要的資訊。 |
| /EXE | 針對經常讀取但未修改的可執行檔，使用優化的壓縮。 支援的演算法為：<ul><li>**XPRESS4K** (最快和預設值) </li><li>**XPRESS8K**</li><li>**XPRESS16K**</li><li>**LZX** (最精簡的) </li></ul> |
| /CompactOs | 設定或查詢系統的壓縮狀態。 支援的選項為：<ul><li>**查詢** -查詢系統的 **Compact** 狀態。</li><li>**一律** 壓縮所有作業系統二進位檔，並將系統狀態設定為 Compact，除非系統管理員變更它。</li><li>**絕不會** Uncompresses 所有的作業系統二進位檔，並將系統狀態設定為非 Compact，除非系統管理員變更它。</li></ul> |
| /windir | 在查詢離線作業系統時，與 **/CompactOs： query** 參數搭配使用。 指定安裝 Windows 的目錄。 |
| `<filename>` | 指定模式、檔案或目錄。 您可以使用多個檔案名，以及 **&#42;** 和 **？** 萬用字元。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 此命令是 NTFS 檔案系統壓縮功能的命令列版本。 目錄的壓縮狀態指出當檔案新增至目錄時，是否要自動壓縮檔案。 設定目錄的壓縮狀態並不一定會變更已在目錄中之檔案的壓縮狀態。

- 您無法使用此命令來讀取、寫入或掛接使用「磁碟空間」或 DoubleSpace 壓縮的磁片區。 您也無法使用此命令來壓縮檔案分配表 (FAT) 或 FAT32 磁碟分割。

## <a name="examples"></a>範例

若要設定目前目錄、其子目錄和現有檔案的壓縮狀態，請輸入：

```
compact /c /s
```

若要設定目前目錄內檔案和子目錄的壓縮狀態，而不改變目前目錄本身的壓縮狀態，請輸入：

```
compact /c /s *.*
```

若要壓縮磁片區，請在磁片區的根目錄中輸入：

```
compact /c /i /s:\
```

> [!NOTE]
> 此範例會設定所有目錄的壓縮狀態 (包括磁片區上的根目錄) 並壓縮磁片區上的每個檔案。 **/I** 參數可防止錯誤訊息中斷壓縮進程。

若要在 \tmp 目錄和 \tmp 的所有子目錄中壓縮具有 .bmp 副檔名的所有檔案，而不修改目錄的壓縮屬性，請輸入：

```
compact /c /s:\tmp *.bmp
```

若要強制完整壓縮檔案 *zebra.bmp*（在系統損毀期間部分壓縮），請輸入：

```
compact /c /f zebra.bmp
```

若要從目錄 c:\tmp 中移除壓縮的屬性，而不變更該目錄中任何檔案的壓縮狀態，請輸入：

```
compact /u c:\tmp
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
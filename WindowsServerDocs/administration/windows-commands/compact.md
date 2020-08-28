---
title: compact
description: Compact 命令的參考文章，可顯示或改變 NTFS 磁碟分割上的檔案或目錄壓縮。
ms.topic: reference
ms.assetid: 429b3752-df0a-43a4-a210-df2f3ad03c3b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 210aaf8c20741659bb29d4855ae39099c964a400
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025942"
---
# <a name="compact"></a>compact

顯示或改變 NTFS 磁碟分割上的檔案或目錄壓縮。 如果使用時不含參數， **compact** 會顯示目前的目錄和其所包含檔案的壓縮狀態。

## <a name="syntax"></a>語法

```
compact [/c | /u] [/s[:<dir>]] [/a] [/i] [/f] [/q] [<filename>[...]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /C | 壓縮指定的目錄或檔案。 |
| /U | Uncompresses 指定的目錄或檔案。 |
| /s [： `<dir>` ] | 將 **compact** 命令套用至指定目錄的所有子目錄 (或目前目錄（如果未指定) ）。 |
| /a | 顯示隱藏的或系統檔案。 |
| /i | 忽略錯誤。 |
| /f | 強制壓縮或解壓縮指定的目錄或檔案。 當系統損毀中斷作業時，會使用 **/f**做為部分壓縮檔案的大小寫。 若要強制壓縮檔案，請使用 **/c** 和 **/f** 參數，並指定部分壓縮檔案。 |
| /q | 只報告最重要的資訊。 |
| `<filename>` | 指定檔案或目錄。 您可以使用多個檔案名，以及 **&#42;** 和 **？** 萬用字元。 |
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
> 此範例會設定所有目錄的壓縮狀態 (包括磁片區上的根目錄) 並壓縮磁片區上的每個檔案。 **/I**參數可防止錯誤訊息中斷壓縮進程。

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
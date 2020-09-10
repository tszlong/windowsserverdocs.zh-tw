---
title: rd
description: Rd 命令的參考文章，此命令會刪除目錄。
ms.topic: reference
ms.assetid: 42e672f6-5bc2-4c16-af25-18e7ed2dd555
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 41e5b22d089a07f5248bece208b04a123a3f3f17
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637356"
---
# <a name="rd"></a>rd

刪除目錄。

**Rd**命令也可以使用不同的參數，從 Windows 修復主控台執行。 如需詳細資訊，請參閱 [Windows 修復環境 (WinRE) ](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)。

> [!NOTE]
> 此命令與 [rmdir 命令](rmdir.md)相同。

## <a name="syntax"></a>語法

```
rd [<drive>:]<path> [/s [/q]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `[<drive>:]<path>` | 指定您想要刪除之目錄的位置和名稱。 需要*路徑*。 如果您在 \) 指定 *路徑*的開頭包含反斜線 (，則 *路徑* 會在根目錄 (開始，無論目前目錄) 為何。 |
| /s | 刪除指定目錄及其所有子目錄 (的目錄樹狀結構，包括) 的所有檔案。 |
| /q | 指定安靜模式。 刪除樹狀目錄時，不會提示確認。 **/Q**參數只有在同時指定 **/s**時才能運作。<p>**注意：** 當您以無訊息模式執行時，系統會刪除整個目錄樹狀結構，而不進行確認。 使用 **/q** 命令列選項之前，請務必移動或備份重要的檔案。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 您無法刪除包含檔案的目錄，包括隱藏的檔案或系統檔案。 如果您嘗試這麼做，則會出現下列訊息：

    `The directory is not empty`

    使用 **dir/a** 命令列出所有檔案 (包括隱藏和系統檔案) 。 然後使用 **attrib** 命令搭配 **-h** 來移除隱藏的檔案屬性、 **-s** 以移除系統檔案屬性，或使用 **-h-s** 來移除隱藏的和系統檔案屬性。 移除隱藏的和檔案屬性之後，您就可以刪除這些檔案。

- 您無法使用 **rd** 命令刪除目前的目錄。 如果您嘗試刪除目前的目錄，則會出現下列錯誤訊息：

    `The process can't access the file because it is being used by another process.`

    如果您收到此錯誤訊息，您必須變更至不同的目錄， (不是目前目錄) 的子目錄，然後再試一次。

### <a name="examples"></a>範例

若要變更為上層目錄，讓您可以安全地移除所需的目錄，請輸入：

```
cd ..
```

若要從目前目錄中移除名為 *test* (的目錄及其所有子目錄) 和檔案，請輸入：

```
rd /s test
```

若要以安靜模式執行上述範例，請輸入：

```
rd /s /q test
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

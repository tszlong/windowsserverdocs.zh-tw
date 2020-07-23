---
title: rd
description: Rd 命令的參考文章，它會刪除目錄。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 42e672f6-5bc2-4c16-af25-18e7ed2dd555
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b0ec217756b7453733398dd85709dc607d32c41
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86956350"
---
# <a name="rd"></a>rd

刪除目錄。

**Rd**命令也可以從 Windows 修復主控台使用不同的參數執行。 如需詳細資訊，請參閱[Windows 修復環境（WinRE）](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)。

> [!NOTE]
> 此命令與[rmdir 命令](rmdir.md)相同。

## <a name="syntax"></a>語法

```
rd [<drive>:]<path> [/s [/q]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `[<drive>:]<path>` | 指定您想要刪除之目錄的位置和名稱。 *Path*是必要的。 如果您在 \) 指定*路徑*的開頭包含反斜線（，則*路徑*會從根目錄開始（不論目前目錄為何）。 |
| /s | 刪除樹狀目錄（指定的目錄及其所有子目錄，包括所有檔案）。 |
| /q | 指定無訊息模式。 刪除目錄樹狀結構時，不會提示您確認。 **/Q**參數只有在同時指定 **/s**時才適用。<p>**注意：** 當您在無訊息模式中執行時，會刪除整個目錄樹狀結構而不進行確認。 使用 **/q**命令列選項之前，請確定重要的檔案已移動或已備份。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 您無法刪除包含檔案的目錄，包括隱藏的檔案或系統檔案。 如果您嘗試這麼做，將會出現下列訊息：

    `The directory is not empty`

    使用**dir/a**命令來列出所有檔案（包括隱藏和系統檔案）。 然後，使用 [ **attrib** ] 命令搭配 **-h**來移除隱藏的檔案屬性、 **-s**以移除系統檔案屬性，或 **-h-s**以移除隱藏的和系統檔案屬性。 移除隱藏的和檔案屬性之後，您就可以刪除檔案。

- 您無法使用**rd**命令刪除目前的目錄。 如果您嘗試刪除目前的目錄，則會出現下列錯誤訊息：

    `The process can't access the file because it is being used by another process.`

    如果您收到此錯誤訊息，您必須變更為不同的目錄（不是目前的目錄的子目錄），然後再試一次。

### <a name="examples"></a>範例

若要變更為上層目錄，讓您可以安全地移除所需的目錄，請輸入：

```
cd ..
```

若要從目前目錄中移除名為*test* （及其所有子目錄和檔案）的目錄，請輸入：

```
rd /s test
```

若要以安靜模式執行上述範例，請輸入：

```
rd /s /q test
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

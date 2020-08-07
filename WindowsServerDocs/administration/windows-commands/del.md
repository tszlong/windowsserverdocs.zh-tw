---
title: del
description: Del 命令的參考文章，它會刪除一個或多個檔案。
ms.topic: article
ms.assetid: 346eede2-2085-44f5-9936-6877b5d5a833
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f909a212284f29fff15de0d02995b08b713cd889
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891500"
---
# <a name="del"></a>del

刪除一或多個檔案。 此命令會執行與**清除**命令相同的動作。

**Del**命令也可以從 Windows 修復主控台使用不同的參數執行。 如需詳細資訊，請參閱[Windows 修復環境 (WinRE) ](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)。

> [!WARNING]
> 如果您使用**del**來刪除磁片中的檔案，就無法取出檔案。

## <a name="syntax"></a>語法

```
del [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
erase [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<names>` | 指定一或多個檔案或目錄的清單。 萬用字元可用來刪除多個檔案。 如果指定目錄，則會刪除目錄中的所有檔案。 |
| /p | 在刪除指定的檔案之前提示您確認。 |
| /f | 強制刪除唯讀檔案。 |
| /s | 從目前的目錄和所有子目錄中刪除指定的檔案。 顯示刪除檔案時的名稱。 |
| /q | 指定無訊息模式。 系統不會提示您進行刪除確認。 |
| /a [：]`<attributes>` | 根據下列檔案屬性刪除檔案：<ul><li>**r**唯讀檔案</li><li>**h**隱藏的檔案</li><li>**我**不是內容索引檔案</li><li>**s**系統檔案</li><li>準備**封存的檔案**</li><li>**l**重新分析點</li><li>**-** 用來做為前置詞，表示 ' not '</li></ul>. |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您使用 `del /p` 命令，您會看到下列訊息：

    `FileName, Delete (Y/N)?`

    若要確認刪除，請按下**Y**。若要取消刪除並顯示下一個檔案名 (如果您) 指定一組檔案，請按**N**。若要停止**del**命令，請按 CTRL + C。

- 如果您停用命令延伸模組， **/s**參數會顯示找不到的任何檔案名，而不是顯示正在刪除之檔案的名稱。

- 如果您在參數中指定特定 `<names>` 的資料夾，則也會一併刪除所有包含的檔案。 例如，如果您想要刪除*\work*資料夾中的所有檔案，請輸入：

  ```
  del \work
  ```

- 您可以使用萬用字元 (**&#42;** 和 **？**) 一次刪除一個以上的檔案。 不過，若要避免不慎刪除檔案，您應該謹慎使用萬用字元。 例如，如果您輸入下列命令：

  ```
  del *.*
  ```

  **Del**命令會顯示下列提示：

  `Are you sure (Y/N)?`

  若要刪除目前目錄中的所有檔案，請按下**Y** ，然後按 enter 鍵。 若要取消刪除，請按**N** ，然後按 enter 鍵。

  > [!NOTE]
  > 使用萬用字元搭配**del**命令之前，請使用與**dir**命令相同的萬用字元來列出將要刪除的所有檔案。

## <a name="examples"></a>範例

若要刪除磁片磁碟機 C 上名為 Test 的資料夾中的所有檔案，請輸入下列其中一項：

```
del c:\test
del c:\test\*.*
```

若要刪除目前目錄中副檔名為 .bat 的所有檔案，請輸入：

```
del *.bat
```

若要刪除目前目錄中的所有唯讀檔案，請輸入：

```
del /a:r *.*
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Windows 修復環境 (WinRE) ](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)

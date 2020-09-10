---
title: erase
description: 清除命令的參考文章，會刪除一或多個檔案。
ms.topic: reference
ms.assetid: 024a4d0f-8679-4e06-b46f-61fdaf5464bc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5545e63efc87527506704ecd6ff956c8000b95a5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636094"
---
# <a name="erase"></a>erase

刪除一個或多個檔案。 如果您使用 **[清除** ] 刪除磁片中的檔案，就無法取出檔案。

> [!NOTE]
> 此命令與 [del 命令](del.md)相同。


## <a name="syntax"></a>語法

```
erase [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
del [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<names>` | 指定一或多個檔案或目錄的清單。 您可以使用萬用字元來刪除多個檔案。 如果指定目錄，則會刪除目錄內的所有檔案。 |
| /p | 在刪除指定的檔案之前提示您確認。 |
| /f | 強制刪除唯讀檔案。 |
| /s | 從目前目錄和所有子目錄中刪除指定的檔案。 顯示正在刪除之檔案的名稱。 |
| /q | 指定安靜模式。 系統不會提示您確認刪除。 |
| /a [：]`<attributes>` | 根據下列檔案屬性刪除檔案：<ul><li>**r** 唯讀檔案</li><li>**h** 隱藏的檔案</li><li>**我** 沒有內容索引檔案</li><li>**s** 系統檔案</li><li>準備**封存的檔案**</li><li>**l** 重新分析點</li><li>**-** 用做為前置詞，表示「不」</li></ul>. |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您使用 `erase /p` 命令，將會看到下列訊息：

    `FileName, Delete (Y/N)?`

    若要確認刪除，請按 **Y**鍵。若要取消刪除並顯示下一個檔案名 (指定) 的檔案群組時，請按 **N**。若要停止 **清除** 命令，請按 CTRL + C。

- 如果您停用了命令延伸模組， **/s** 參數將會顯示找不到的任何檔案名，而不是顯示正在刪除之檔案的名稱。

- 如果您在參數中指定特定資料夾 `<names>` ，則也會一併刪除所有包含的檔案。 例如，如果您想要刪除 *\work* 資料夾中的所有檔案，請輸入：

  ```
  erase \work
  ```

- 您可以使用萬用字元 (**&#42;** 和 **？**) 一次刪除一個以上的檔案。 不過，為了避免不慎刪除檔案，您應該謹慎使用萬用字元。 例如，如果您輸入下列命令：

  ```
  erase *.*
  ```

  **清除**命令會顯示下列提示：

  `Are you sure (Y/N)?`

  若要刪除目前目錄中的所有檔案，請按 **Y** ，然後按 enter。 若要取消刪除，請按 **N** ，然後按 enter。

  > [!NOTE]
  > 使用萬用字元搭配 **清除** 命令之前，請使用與 **dir** 命令相同的萬用字元來列出將刪除的所有檔案。

### <a name="examples"></a>範例

若要刪除磁片磁碟機 C 上名為 Test 之資料夾中的所有檔案，請輸入下列其中一項：

```
erase c:\test
erase c:\test\*.*
```

若要從目前的目錄中刪除副檔名為 .bat 的所有檔案，請輸入：

```
erase *.bat
```

若要刪除目前目錄中的所有唯讀檔案，請輸入：

```
erase /a:r *.*
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [del 命令](del.md)
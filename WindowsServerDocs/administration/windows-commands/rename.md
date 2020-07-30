---
title: 重新命名
description: Rename 命令的參考文章，會重新命名檔案或目錄。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7f2ea658-0fa9-4015-8031-22c2b0089231
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cf7a962a83b7cf8f00ea4963e358c0329ae28c3b
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87409659"
---
# <a name="rename"></a>重新命名

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

重新命名檔案或目錄。

> [!NOTE]
> 此命令與[ren 命令](ren.md)相同。

## <a name="syntax"></a>語法

```
rename [<drive>:][<path>]<filename1> <filename2>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `[<drive>:][<path>]<filename1>` | 指定要重新命名之檔案或檔案集的位置和名稱。 *Filename1*可以包含萬用字元（**&#42;** 和 **？**）。 |
| `<filename2>` | 指定檔案的新名稱。 您可以使用萬用字元來指定多個檔案的新名稱。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 您無法在重新命名檔案時指定新的磁片磁碟機或路徑。 您也不能使用此命令來重新命名磁片磁碟機上的檔案，或將檔案移至不同的目錄。

- *Filename2*中以萬用字元表示的字元將與*filename1*中的對應字元相同。

- *Filename2*必須是唯一的檔案名。 如果*filename2*符合現有的檔案名，則會出現下列訊息： `Duplicate file name or file not found` 。

### <a name="examples"></a>範例

若要將目前目錄中的所有 .txt 副檔名變更為 .doc 副檔名，請輸入：

```
rename *.txt *.doc
```

若要將目錄的名稱從*Chap10*變更為*Part10*，請輸入：

```
rename chap10 part10
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [ren 命令](ren.md)

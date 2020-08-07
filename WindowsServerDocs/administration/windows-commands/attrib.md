---
title: attrib
description: Attrib 命令的參考文章，它會顯示、設定或移除指派給檔案或目錄的屬性。
ms.topic: article
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73651c85ac1cc35c54845ffe207cea3f0887cd8f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895503"
---
# <a name="attrib"></a>attrib

顯示、設定或移除指派給檔案或目錄的屬性。 如果使用時不含參數，則**attrib**會顯示目前目錄中所有檔案的屬性。

## <a name="syntax"></a>語法

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<drive>:][<path>][<filename>] [/s [/d] [/l]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `{+|-}r` | 設定 (**+**) 或清除 **-** 唯讀檔案屬性)  (。 |
| `{+\|-}a` | 設定 (**+**) 或清除封存檔案 **-** 屬性)  (。 此屬性集會標記自從上次備份後已變更的檔案。 請注意， **xcopy**命令會使用封存屬性。 |
| `{+\|-}s` | 設定 (**+**) 或清除 **-** 系統檔案屬性)  (。 如果檔案使用此屬性集，您必須先清除該屬性，才能變更該檔案的任何其他屬性。 |
| `{+\|-}h` | 設定 (**+**) 或清除 **-** 隱藏檔案屬性)  (。 如果檔案使用此屬性集，您必須先清除該屬性，才能變更該檔案的任何其他屬性。 |
| `{+\|-}i` | 設定 (**+**) 或清除 **-** [非內容] 索引檔案屬性 () 。 |
| `[<drive>:][<path>][<filename>]` | 指定您要顯示或變更屬性的目錄、檔案或檔案群組的位置和名稱。<p>您可以使用 **？** 和會在*filename*參數中 **&#42;** 萬用字元，以顯示或變更一組檔案的屬性。 |
| /s | 將**attrib**和任何命令列選項套用至目前目錄及其所有子目錄中的相符檔案。 |
| /d | 將**attrib**和任何命令列選項套用至目錄。 |
| /l | 將**attrib**和任何命令列選項套用至符號連結，而不是符號連結的目標。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要顯示位於目前目錄中名為 News86 之檔案的屬性，請輸入：

```
attrib news86
```

若要將唯讀屬性指派給名為 report.txt 的檔案，請輸入：

```
attrib +r report.txt
```

若要從公用目錄中的檔案，以及磁片磁碟機 b：中磁片上的子目錄移除唯讀屬性，請輸入：

```
attrib -r b:\public\*.* /s
```

若要設定磁片磁碟機 a：上所有檔案的封存屬性，然後清除副檔名為 .bak 之檔案的封存屬性，請輸入：

```
attrib +a a:*.* & attrib -a a:*.bak
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [xcopy 命令](xcopy.md)

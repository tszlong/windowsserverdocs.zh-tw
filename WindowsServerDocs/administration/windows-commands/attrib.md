---
title: attrib
description: Attrib 命令的參考文章，可顯示、設定或移除指派給檔案或目錄的屬性。
ms.topic: reference
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1bf34ad853421d395a76e27313d92330922ba2d0
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633314"
---
# <a name="attrib"></a>attrib

顯示、設定或移除指派給檔案或目錄的屬性。 如果使用時不含參數，則 **attrib** 會顯示目前的目錄中所有檔案的屬性。

## <a name="syntax"></a>語法

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<drive>:][<path>][<filename>] [/s [/d] [/l]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `{+|-}r` | 設定 (**+**) ，或清除 **-** 唯讀檔案屬性)  (。 |
| `{+\|-}a` | 設定 (**+**) ，或清除 [封存檔案] **-** 屬性)  (。 這個屬性集會標示自從上次備份後已變更的檔案。 請注意， **xcopy** 命令會使用封存屬性。 |
| `{+\|-}s` | 設定 (**+**) ，或清除 **-** 系統檔案屬性)  (。 如果檔案使用這個屬性集，您必須先清除屬性，才能變更檔案的任何其他屬性。 |
| `{+\|-}h` | 設定 (**+**) ，或清除 **-** 隱藏的檔案屬性)  (。 如果檔案使用這個屬性集，您必須先清除屬性，才能變更檔案的任何其他屬性。 |
| `{+\|-}i` | 設定 (**+**) ，或清除 (**-**) 非內容索引的檔案屬性。 |
| `[<drive>:][<path>][<filename>]` | 指定要顯示或變更屬性的目錄、檔案或檔案群組的位置和名稱。<p>您可以使用 **？** 和在*filename*參數中 **&#42;** 萬用字元，以顯示或變更檔案群組的屬性。 |
| /s | 將 **attrib** 和任何命令列選項套用至目前目錄及其所有子目錄中的相符檔案。 |
| /d | 將 **attrib** 和任何命令列選項套用至目錄。 |
| /l | 將 **attrib** 和任何命令列選項套用至符號連結，而不是符號連結的目標。 |
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

若要從公用目錄中的檔案和磁片磁碟機 b：磁片中的子目錄移除唯讀屬性，請輸入：

```
attrib -r b:\public\*.* /s
```

若要為磁片磁碟機 a：上的所有檔案設定 Archive 屬性，然後針對副檔名為 .bak 的檔案清除 [封存] 屬性，請輸入：

```
attrib +a a:*.* & attrib -a a:*.bak
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [xcopy 命令](xcopy.md)

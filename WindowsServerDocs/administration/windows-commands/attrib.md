---
title: attrib
description: 適用于**attrib**的 Windows 命令主題，它會顯示、設定或移除指派給檔案或目錄的屬性。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94a6f307450b06dff81180b70f864bd9e1ed5885
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851251"
---
# <a name="attrib"></a>attrib

顯示、設定或移除指派給檔案或目錄的屬性。 如果使用時不含參數，則**attrib**會顯示目前目錄中所有檔案的屬性。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<Drive>:][<Path>][<FileName>] [/s [/d] [/l]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `{+|-}r` | 設定（ **+** ）或清除（ **-** ）唯讀檔案屬性。 |
| `{+\|-}a` | 設定（ **+** ）或清除（ **-** ）封存檔案屬性。 |
| `{+\|-}s` | 設定（ **+** ）或清除（ **-** ）系統檔案屬性。 |
| `{+\|-}h` | 設定（ **+** ）或清除（ **-** ）隱藏檔案屬性。 |
| `{+\|-}i` | 設定（ **+** ）或清除（ **-** ） [非內容索引] 檔案屬性。 |
| `[<Drive>:][<Path>][<FileName>]` | 指定您要顯示或變更屬性的目錄、檔案或檔案群組的位置和名稱。 您可以使用 **？** FileName **&#42;** 參數中的和*FileName*萬用字元，用來顯示或變更一組檔案的屬性。 |
| /s | 將**attrib**和任何命令列選項套用至目前目錄及其所有子目錄中的相符檔案。 |
| /d | 將**attrib**和任何命令列選項套用至目錄。 |
| /l | 將**attrib**和任何命令列選項套用至符號連結，而不是符號連結的目標。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

- 您可以使用萬用字元（ **？** 和 **&#42;** ）搭配*FileName*參數，以顯示或變更一組檔案的屬性。

- 如果檔案已設定 [系統] 或 [隱藏（**h** **）]** 屬性，您必須先清除該屬性，才能變更該檔案的任何其他屬性。

- 封存屬性（**a**）會標記自從上次備份後已變更的檔案。 請注意， **xcopy**命令會使用封存屬性。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要顯示位於目前目錄中名為 News86 之檔案的屬性，請輸入：

```
attrib news86 
```

若要將唯讀屬性指派給名為 Report .txt 的檔案，請輸入：

```
attrib +r report.txt 
```

若要從公用目錄中的檔案及其子目錄中的磁片磁碟機 B 中移除唯讀屬性，請輸入：

```
attrib -r b:\public\*.* /s 
```

若要設定磁片磁碟機 A 上所有檔案的封存屬性，然後清除副檔名為 .bak 之檔案的封存屬性，請輸入：

```
attrib +a a:*.* & attrib -a a:*.bak 
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
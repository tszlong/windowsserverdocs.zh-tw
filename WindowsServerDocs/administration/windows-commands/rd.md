---
title: rd
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 42e672f6-5bc2-4c16-af25-18e7ed2dd555
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cb169a44f9613b237af71321f9619d9ea93a6912
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722638"
---
# <a name="rd"></a>rd



刪除目錄。 此命令與**rmdir**命令相同。



## <a name="syntax"></a>語法

```
rd [<Drive>:]<Path> [/s [/q]]
rmdir [<Drive>:]<Path> [/s [/q]]
```

### <a name="parameters"></a>參數

|     參數     |                                                                 描述                                                                  |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| [\<磁片磁碟機>：]<Path> |                      指定您想要刪除之目錄的位置和名稱。 *Path*是必要的。                       |
|        /s         |                     刪除樹狀目錄（指定的目錄及其所有子目錄，包括所有檔案）。                      |
|        /q         | 指定無訊息模式。 刪除目錄樹狀結構時，不會提示您確認。 （請注意， **/q**只有在指定 **/s**時才會運作）。 |
|        /?         |                                                     在命令提示字元顯示說明。                                                     |

## <a name="remarks"></a>備註

-   您無法刪除包含檔案的目錄，包括隱藏的檔案或系統檔案。 如果您嘗試這麼做，將會出現下列訊息：

    `The directory is not empty`

    使用**dir/a**命令來列出所有檔案（包括隱藏和系統檔案）。 然後，使用 [ **attrib** ] 命令搭配 **-h**來移除隱藏的檔案屬性、 **-s**以移除系統檔案屬性，或 **-h-s**以移除隱藏的和系統檔案屬性。 移除隱藏的和檔案屬性之後，您就可以刪除檔案。
-   如果您插入反斜線（\)在*路徑*的開頭，*路徑*會從根目錄開始（不論目前目錄為何）。
-   您無法使用**rd**刪除目前的目錄。 如果您嘗試刪除目前的目錄，則會出現下列錯誤訊息：

    `The process cannot access the file because it is being used by another process.`

    如果您收到此錯誤訊息，您必須變更為不同的目錄（不是目前的目錄的子目錄），然後使用**rd** （如有必要，請指定*路徑*）。
-   您可以從修復主控台使用具有不同參數的**rd**命令。

## <a name="examples"></a>範例

您無法刪除目前正在使用的目錄。 您必須變更為不在目前目錄內的目錄。 例如，若要變更為上層目錄，請輸入：
```
cd ..
```
您現在可以安全地移除所需的目錄。

使用 **/s**選項來移除目錄樹狀結構。 例如，若要從目前目錄中移除名為 Test （及其所有子目錄和檔案）的目錄，請輸入：
```
rd /s test
```
若要以安靜模式執行上述範例，請輸入：
```
rd /s /q test
```

> [!CAUTION]
> 當您在無訊息模式中執行**rd/s**時，會刪除整個目錄樹狀結構而不進行確認。 使用 **/q**命令列選項之前，請確定重要的檔案已移動或已備份。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
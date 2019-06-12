---
title: rd
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 42e672f6-5bc2-4c16-af25-18e7ed2dd555
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94231e3ec032280beb91a14db7949a1296c2d811
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442035"
---
# <a name="rd"></a>rd



刪除的目錄。 此命令等同於**rmdir**命令。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
rd [<Drive>:]<Path> [/s [/q]]
rmdir [<Drive>:]<Path> [/s [/q]]
```

## <a name="parameters"></a>參數

|     參數     |                                                                 描述                                                                  |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive>:]<Path> |                      指定的位置和您想要刪除的目錄名稱。 *路徑*需要。                       |
|        /s         |                     刪除樹狀目錄 （指定的目錄和所有子目錄，包括所有檔案）。                      |
|        /q         | 指定無訊息模式。 不會提示進行確認，當您刪除樹狀目錄時。 (請注意， **/q** works 才 **/s**指定。) |
|        /?         |                                                     在命令提示字元顯示說明。                                                     |

## <a name="remarks"></a>備註

-   您無法刪除此目錄包含檔案，包括隱藏或系統檔案。 如果您嘗試這樣做，就會出現下列訊息：

    `The directory is not empty`

    使用**dir /a**命令來列出所有檔案 (包括隱藏檔案和系統檔案)。 然後使用**attrib**命令搭配 **-h**移除隱藏的檔案屬性 **-s**移除系統檔案屬性，或 **-h-s**移除兩者都隱藏和系統檔案屬性。 之後隱藏並已移除檔案屬性，您可以刪除的檔案。
-   如果您要插入反斜線 (\)開頭*路徑*，*路徑*時會啟動 （不論目前的目錄） 的根目錄。
-   您無法使用**rd**刪除目前的目錄。 如果您嘗試刪除目前的目錄，則會出現下列錯誤訊息：

    `The process cannot access the file because it is being used by another process.`

    如果您收到這個錯誤訊息，您必須切換至不同的目錄 （不是目錄的子目錄目前），並用**rd** (指定*路徑*如有必要)。
-   **Rd**命令，使用不同的參數，可從修復主控台。

## <a name="BKMK_examples"></a>範例

您無法刪除您目前使用中的目錄。 您必須變更為不在目前的目錄內的目錄。 例如，若要變更的父目錄，請輸入：
```
cd ..
```
您現在可以安全地移除所需的目錄。

使用 **/s**選項，以移除目錄樹狀結構。 比方說，若要移除目錄命名測試 （和所有其子目錄和檔案） 從目前的目錄，類型：
```
rd /s test
```
若要以無訊息模式執行前一個範例，請輸入：
```
rd /s /q test
```

> [!CAUTION]
> 當您執行**rd /s**在無訊息模式中，不經過確認刪除整個目錄樹狀結構。 確保重要的檔案已移動或備份，才能使用 **/q**命令列選項。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
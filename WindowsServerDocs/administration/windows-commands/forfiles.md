---
title: forfiles
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 43f6b004-446d-4fdd-91c5-5653613524a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: fa2eb5c96dfbf3870705af41a0f27991084f816d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377074"
---
# <a name="forfiles"></a>forfiles



選取並執行檔案或一組檔案的命令。 此命令適用于批次處理。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
forfiles [/p <Path>] [/m <SearchMask>] [/s] [/c "<Command>"] [/d [{+|-}][{<Date>|<Days>}]]
```


## <a name="parameters"></a>參數

|                     參數                      |                                                                                                                                                                                                                                                                                                    描述                                                                                                                                                                                                                                                                                                     |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /p \<Path >                     |                                                                                                                                                                                                                                                 指定要從中開始搜尋的路徑。 根據預設，搜尋會從目前的工作目錄開始。                                                                                                                                                                                                                                                  |
|                  /m \<SearchMask >                  |                                                                                                                                                                                                                                                           根據指定的搜尋遮罩搜尋檔案。 預設搜尋遮罩為 **\*。 \\** \*。                                                                                                                                                                                                                                                           |
|                         /s                         |                                                                                                                                                                                                                                                                   指示**forfiles**命令以遞迴方式搜尋子目錄。                                                                                                                                                                                                                                                                    |
|                  /c "@no__t 0Command >"                   |                                                                                                                                                                                                                                  在每個檔案上執行指定的命令。 命令字串應該括在引號中。 預設的命令為 **"cmd/c echo @file"** 。                                                                                                                                                                                                                                   |
| /d @ no__t-0 [{+ \|-}]&#8288;[{@no__t 3Date > \|&#8288;\<Days >}] | 選取在指定的時間範圍內，具有上次修改日期的檔案。</br>-選取上次修改日期晚于或等於（ **+** ）或早于或等於（ **-** ）指定日期的檔案，其中*date*的格式為 MM/DD/YYYY。</br>-選取上次修改日期晚于或等於（ **+** ）目前日期加上指定的天數，或早于或等於（ **-** ）目前日期減去指定天數的檔案。</br>- *Days*的有效值包括0–32768範圍內的任何數位。 如果未指定正負號，預設會使用 **+** 。 |
|                         /?                         |                                                                                                                                                                                                                                                                                        在命令提示字元顯示說明。                                                                                                                                                                                                                                                                                        |

## <a name="remarks"></a>備註

-   **Forfiles**最常用於批次檔中。
-   **Forfiles/s**類似**dir/s.**
-   您可以在命令字串中使用下列變數，如 **/c**命令列選項所指定。  

|變數|描述|
|--------|-----------|
|@FILE|檔案名。|
|@FNAME|不含副檔名的檔案名。|
|@EXT|檔案名副檔名。|
|@PATH|檔案的完整路徑。|
|@RELPATH|檔案的相對路徑。|
|@ISDIR|如果檔案類型是目錄，則評估結果為 TRUE。 否則，此變數會評估為 FALSE。|
|@FSIZE|檔案大小（以位元組為單位）。|
|@FDATE|上次修改檔案的日期戳記。|
|@FTIME|上次修改檔案的時間戳記。|

-   透過**forfiles**，您可以在上執行命令，或將引數傳遞至多個檔案。 例如，您可以在具有 .txt 副檔名的樹狀目錄中的所有檔案上執行**type**命令。 或者，您也可以在磁片磁碟機 C 上執行每個批次檔（* .bat），檔案名為 "Myinput.x .txt" 做為第一個引數。
-   透過**forfiles**，您可以執行下列任何一項動作：  
    -   使用 **/d**參數選取 [檔案] [絕對日期] 或 [相對日期]。
    -   使用 @FSIZE 和 @FDATE 之類的變數，建立檔案的封存樹狀目錄。
    -   使用 @no__t 0 變數來區分目錄中的檔案。
    -   在命令列中包含特殊字元，方法是使用字元的十六進位代碼，格式為 0x*HH* （例如，索引標籤的0x09）。
-   **Forfiles**的運作方式是在設計用來處理單一檔案的工具上，執行**遞迴子目錄**旗標。

## <a name="BKMK_examples"></a>典型

若要列出 C 磁片磁碟機上的所有批次檔，請輸入：
```
forfiles /p c:\ /s /m *.bat /c "cmd /c echo @file is a batch file"
```
若要列出 C 磁片磁碟機上的所有目錄，請輸入：
```
forfiles /p c:\ /s /m *.* /c "cmd /c if @isdir==TRUE echo @file is a directory"
```
若要列出目前目錄中至少一年以前的所有檔案，請輸入：
```
forfiles /s /m *.* /d -365 /c "cmd /c echo @file is at least one year old."
```
若要針對目前目錄中，2007年1月1日以前的每個檔案顯示「*檔案已過時*」文字，請輸入：
```
forfiles /s /m *.* /d -01/01/2007 /c "cmd /c echo @file is outdated." 
```
若要以欄格式列出目前目錄中所有檔案的副檔名，並在擴充功能前面加上索引標籤，請輸入：
```
forfiles /s /m *.* /c "cmd /c echo The extension of @file is 0x09@ext" 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

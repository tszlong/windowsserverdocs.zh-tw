---
title: diskcopy
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fd21efa-52cc-4e70-a7fe-35125a435106
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/07/2018
ms.openlocfilehash: aadb3a77cda7f1403cd2f04ced12c17617f046df
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439576"
---
# <a name="diskcopy"></a>diskcopy



來源磁碟機的磁碟片的內容複製至格式化或未格式化磁片中的目的地磁碟機中。 如果未指定參數，使用**diskcopy**來源磁碟的目的地磁碟會使用目前的磁碟機。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

> [!NOTE]
> 此命令不包含在 Windows 10。

## <a name="syntax"></a>語法

```
diskcopy [<Drive1>: [<Drive2>:]] [/v]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Drive1>|指定包含來源磁碟的磁碟機。|
|\<Drive2>|指定包含目的地磁碟的磁碟機。|
|/v|確認已正確複製資訊。 此選項會降低複製程序。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   使用磁碟

    **Diskcopy**只適用於卸除式磁碟，例如磁碟，必須是相同的型別。 您無法使用**diskcopy**與硬碟。 如果您指定的硬碟*Drive1*或是*Drive2*， **diskcopy**會顯示下列錯誤訊息：  
    ```
    Invalid drive specification
    Specified drive does not exist or is nonremovable
    ```  
    **Diskcopy**命令會提示您插入的來源和目的地磁碟，並等候您按鍵盤上的任何索引鍵，然後再繼續。

    它會複製磁碟之後, **diskcopy**顯示下列訊息：  
    ```
    Copy another diskette (Y/N)?
    ```  
    如果您按下 Y **diskcopy**會提示您插入下一個複製作業的來源和目的地磁碟。 若要停止**diskcopy**程序時，按下**N**。

    如果您複製到中的未格式化磁片*Drive2*， **diskcopy**格式化磁碟，以使用相同數目的側邊和每個使用中的磁碟上盡可能的磁軌的磁區*Drive1*。 **Diskcopy**時它格式化磁碟，並將檔案複製，則會顯示下列訊息：  
    ```
    Formatting while copying
    ```  
-   磁碟序號

    如果來源磁碟有磁碟區序號**diskcopy**會建立新的磁碟區數列數字，目的地磁碟，並複製作業完成時，會顯示數字。
-   省略磁碟機參數

    如果您省略*Drive2*參數**diskcopy**目前的磁碟機做為目的地磁碟機。 如果省略這兩個磁碟機的參數， **diskcopy**同時使用目前的磁碟機。 如果目前的磁碟機是相同*Drive1*， **diskcopy**會提示您視需要交換磁碟。
-   使用一個磁碟機複製

    執行**diskcopy**從以外軟碟機的磁碟機，例如 C 磁碟機。 如果磁碟片*Drive1*和 磁碟片*Drive2*相同， **diskcopy**會提示您在切換磁碟。 如果磁碟包含詳細的資訊可以保存的可用記憶體，比**diskcopy**無法讀取的所有資訊一次。 **Diskcopy**從來源磁碟中讀取、 寫入目的地磁碟，並提示您重新插入來源磁碟。 此程序會繼續直到您複製整個磁碟。
-   避免磁碟分散程度

    分散程度過小的磁碟上的現有檔案之間的未使用的磁碟空間的區域存在。 分散的來源磁碟是以尋找、 讀取或寫入檔案的程序變慢。

    因為**diskcopy**使目的地磁碟的來源磁碟、 來源磁碟上的所有片段的完全相同複本傳輸至目的地磁碟。 若要避免從一部磁碟的分散程度傳輸到另一個，請使用**複製**或是**xcopy**複製您的磁碟。 因為**複製**並**xcopy**複製檔案循序，新的磁碟未分散。

> [!NOTE]
> 您無法使用**xcopy**複製啟動磁碟。
> -   了解**diskcopy**結束代碼

    The following table explains each exit code.  
    |結束代碼|描述|
    |---------|-----------|
    |0|複製作業已順利完成|
    |1|發生非嚴重的讀取/寫入錯誤|
    |3|發生嚴重的硬體錯誤|
    |4|發生初始化錯誤|

    To process the exit codes that are returned by **diskcomp**, you can use the *ERRORLEVEL* environment variable on the **if** command line in a batch program.

## <a name="BKMK_examples"></a>範例

若要將磁碟機 B 中的複製到磁碟機 A 中的磁碟，請輸入：
```
diskcopy b: a:
```
若要使用軟碟機的複製到另一個磁碟，請先切換到 C 磁碟機，然後輸入：

diskcopy a: a:

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
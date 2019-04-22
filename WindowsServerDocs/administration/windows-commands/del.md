---
title: del
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 346eede2-2085-44f5-9936-6877b5d5a833
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1c2756e53d9f047160ddd037b3868e47d6e3181
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822989"
---
# <a name="del"></a>del



刪除一個或多個檔案。 此命令等同於**清除**命令。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
del [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
erase [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<名稱 >|指定一或多個檔案或目錄的清單。 若要刪除多個檔案，可能會使用萬用字元。 如果指定的目錄，則會刪除目錄中的所有檔案。|
|/p|確認再刪除指定的檔案的提示。|
|/f|強制刪除唯讀檔案。|
|/s|刪除指定的檔案從目前的目錄和所有子目錄。 正在刪除會顯示檔案的名稱。|
|/q|指定無訊息模式。 系統不會提示確認刪除。|
|/a[:]\<Attributes>|刪除下列檔案屬性為基礎的檔案：</br>**r**唯讀檔案</br>**h**隱藏檔案</br>**我**不內容索引的檔案</br>**s**系統檔案</br>封存的檔案</br>**l**重新分析點</br>-首碼 'not' 表示|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

> [!CAUTION]
> 如果您使用**del**從磁碟刪除檔案，您無法擷取它。
-   如果您使用 **/p**， **del**顯示檔案的名稱，並將傳送下列訊息：

    `FileName, Delete (Y/N)?`

    若要確認刪除，請按 Y。若要取消刪除和顯示的項目下, 一個檔案名稱 （亦即，如果您指定的檔案群組中），請按 n。若要停止**del**命令中，按 CTRL + C。
-   如果您停用擴充命令，請 **/s**顯示而不是顯示 正在刪除的檔案名稱找不到任何檔案的名稱 （也就是相反的行為）。
-   如果您指定的資料夾中*名稱*，也會刪除所有資料夾中的檔案。 例如，下列命令會刪除所有 \Work 資料夾中的檔案：  
    ```
    del \work
    ```  
-   您可以使用萬用字元 (**&#42;** 並 **？**) 若要刪除多個檔案一次。 不過，若要避免不小心刪除檔案，您應該使用萬用字元小心**del**命令。 例如，如果您輸入下列命令：  
    ```
    del *.*
    ```  
    **Del**命令會顯示下列提示：

    `Are you sure (Y/N)?`

    若要刪除所有目前的目錄中的檔案，請按下 Y 鍵，然後按 ENTER 鍵。 若要取消刪除，按 N 然後按 ENTER。

> [!NOTE]
> 在您使用萬用字元的字元之前**del**命令，使用具有相同的萬用字元**dir**命令，列出將會被刪除的所有檔案。
-   **Del**命令，使用不同的參數，可從修復主控台。

## <a name="BKMK_examples"></a>範例

若要刪除名為 Test C 磁碟機上的資料夾中的所有檔案，請輸入下列其中一項：
```
del c:\test
del c:\test\*.*
```
若要從目前的目錄中刪除所有副檔名為.bat 檔案的檔案，請輸入：
```
del *.bak
```
若要刪除目前目錄中的所有唯讀檔案，請輸入：
```
del /a:r *.*
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
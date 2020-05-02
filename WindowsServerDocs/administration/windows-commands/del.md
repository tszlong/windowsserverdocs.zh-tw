---
title: del
description: Del 的參考主題，會刪除一個或多個檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 346eede2-2085-44f5-9936-6877b5d5a833
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 703597ae422518a5b401b656ace0b4cd73418be8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716751"
---
# <a name="del"></a>del

刪除一或多個檔案。 此命令與 [**清除**] 命令相同。



## <a name="syntax"></a>語法

```
del [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
erase [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<名稱>|指定一或多個檔案或目錄的清單。 萬用字元可用來刪除多個檔案。 如果指定目錄，則會刪除目錄中的所有檔案。|
|/p|在刪除指定的檔案之前提示您確認。|
|/f|強制刪除唯讀檔案。|
|/s|從目前的目錄和所有子目錄中刪除指定的檔案。 顯示刪除檔案時的名稱。|
|/q|指定無訊息模式。 系統不會提示您進行刪除確認。|
|/a [：]\<屬性>|根據下列檔案屬性刪除檔案：</br>**r**唯讀檔案</br>**h**隱藏的檔案</br>**我**不是內容索引檔案</br>**s**系統檔案</br>準備**封存的檔案**</br>**l**重新分析點</br>-前置詞意義 ' not '|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

> [!CAUTION]
> 如果您使用**del**從磁片刪除檔案，就無法抓取檔案。

-   如果您使用 **/p**， **del**會顯示檔案名，並傳送下列訊息：

    `FileName, Delete (Y/N)?`

    若要確認刪除，請按下 Y。若要取消刪除並顯示下一個檔案名（也就是，如果您指定了一組檔案），請按 N。若要停止**del**命令，請按 CTRL + C。
- 如果您停用命令延伸模組， **/s**會顯示找不到的任何檔案名，而不是顯示所要刪除之檔案的名稱（也就是相反的行為）。
- 如果您在 [*名稱*] 中指定資料夾，則會刪除資料夾中的所有檔案。 例如，下列命令會刪除 \Work 資料夾中的所有檔案：  
  ```
  del \work
  ```  
- 您可以使用萬用字元（**&#42;** 和 **？**）一次刪除一個以上的檔案。 不過，若要避免不慎刪除檔案，您應該使用**del**命令來謹慎使用萬用字元。 例如，如果您輸入下列命令：  
  ```
  del *.*
  ```  
  **Del**命令會顯示下列提示：

  `Are you sure (Y/N)?`

  若要刪除目前目錄中的所有檔案，請按下 Y，然後按 ENTER 鍵。 若要取消刪除，請按 N，然後按 ENTER 鍵。

> [!NOTE]
> 使用萬用字元搭配**del**命令之前，請使用與**dir**命令相同的萬用字元來列出將要刪除的所有檔案。

-   具有不同參數的**del**命令可從 [修復主控台] 取得。

## <a name="examples"></a>範例

若要刪除磁片磁碟機 C 上名為 Test 的資料夾中的所有檔案，請輸入下列其中一項：
```
del c:\test
del c:\test\*.*
```
若要刪除目前目錄中副檔名為 .bat 的所有檔案，請輸入：
```
del *.bat
```
若要刪除目前目錄中的所有唯讀檔案，請輸入：
```
del /a:r *.*
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

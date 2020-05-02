---
title: diskcopy
description: Diskcopy 的參考主題，它會將來源磁片磁碟機中的軟碟內容複寫到目的地磁片磁碟機中格式化或未格式化的磁片。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fd21efa-52cc-4e70-a7fe-35125a435106
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/07/2018
ms.openlocfilehash: b5c9186a539a58ed0d3362ba83d7a3bcedcaabad
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719463"
---
# <a name="diskcopy"></a>diskcopy

將來源磁片磁碟機中的軟碟內容複寫到目的地磁片磁碟機中已格式化或未格式化的軟碟。 如果使用時不含參數，則**diskcopy**會使用來源磁片和目的地磁片的目前磁片磁碟機。



> [!NOTE]
> 此命令不包含在 Windows 10 中。

## <a name="syntax"></a>語法

```
diskcopy [<Drive1>: [<Drive2>:]] [/v]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Drive1>|指定包含來源磁片的磁片磁碟機。|
|\<Drive2>|指定包含目的地磁片的磁片磁碟機。|
|/v|確認已正確複製資訊。 此選項會使複製程式變慢。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   使用磁片

    **Diskcopy**僅適用于卸載式磁片（如磁片），其類型必須相同。 您不能將**diskcopy**與硬碟搭配使用。 如果您為*Drive1*或*Drive2*指定硬碟，則**diskcopy**會顯示下列錯誤訊息：  
    ```
    Invalid drive specification
    Specified drive does not exist or is nonremovable
    ```  
    **Diskcopy**命令會提示您插入來源和目的地磁片，並等待您在鍵盤上按下任何鍵，然後再繼續。

    在複製磁片之後， **diskcopy**會顯示下列訊息：  
    ```
    Copy another diskette (Y/N)?
    ```  
    如果您按下 Y，則**diskcopy**會提示您插入下一個複製作業的來源和目的地磁片。 若要停止**diskcopy**進程，請按**N**。

    如果您要在*Drive2*中複製到未格式化的磁片磁碟機，則**diskcopy**會將每個磁軌的相同邊和磁區數目格式化磁片，如同*Drive1*中的磁片。 **Diskcopy**會在格式化磁片並複製檔案時顯示下列訊息：  
    ```
    Formatting while copying
    ```  
-   磁片序號

    如果來源磁片具有磁片區序號， **diskcopy**會為目的地磁片建立新的磁片區序號，並在複製作業完成時顯示該數位。
-   省略磁片磁碟機參數

    如果您省略*Drive2*參數， **diskcopy**會使用目前的磁片磁碟機做為目的地磁片磁碟機。 如果您省略這兩個磁片磁碟機參數， **diskcopy**會同時使用目前的磁片磁碟機。 如果目前的磁片磁碟機與*Drive1*相同，則**diskcopy**會提示您視需要交換磁片。
-   使用一個磁片磁碟機進行複製

    從軟碟磁碟機以外的磁片磁碟機（例如 C 磁片磁碟機）執行**diskcopy** 。 如果磁片磁碟機*Drive1*和磁片*Drive2*相同，則**diskcopy**會提示您切換磁片。 如果磁片包含的資訊超過可用記憶體的可能，則**diskcopy**無法一次讀取所有資訊。 **Diskcopy**讀取來源磁片，寫入目的地磁片，並提示您重新插入來源磁片。 此程式會繼續進行，直到您已複製整個磁片為止。
-   避免磁碟片段

    片段是磁片上的現有檔案之間是否有未使用磁碟空間的小型區域。 分割的來源磁片可能會使尋找、讀取或寫入檔案的程式變慢。

    因為**diskcopy**會在目的地磁片上建立完全相同的來源磁片複本，所以來源磁片上的任何片段都會傳輸到目的地磁片。 若要避免將片段從一個磁片傳輸到另一個磁片，請使用**copy**或**xcopy**來複製磁片。 由於**copy**和**xcopy**會依序複製檔案，因此不會分割新的磁片。

> [!NOTE]
> 您不能使用**xcopy**來複製啟動磁片。

### <a name="understanding-diskcopy-exit-codes"></a>瞭解**diskcopy**結束代碼

    The following table explains each exit code.
    
    |結束代碼|描述|
    |---------|-----------|
    |0|複製操作成功|
    |1|發生非嚴重的讀取/寫入錯誤|
    |3|發生嚴重硬體錯誤|
    |4|發生初始化錯誤|

    To process the exit codes that are returned by **diskcomp**, you can use the *ERRORLEVEL* environment variable on the **if** command line in a batch program.

## <a name="examples"></a>範例

若要將磁片磁碟機 B 中的磁碟複製到磁片磁碟機 A 中的磁片，請輸入：
```
diskcopy b: a:
```
若要使用軟碟磁碟機 A 將磁碟複製到另一張軟碟，請先切換至 C 磁片磁碟機，然後輸入：

diskcopy a： a：

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
---
title: ren
description: 瞭解如何使用 ren 命令來重新命名檔案或目錄。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60398e12-a05d-4524-a73a-0a925943e21d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 21ce37795af3080245633938e96f891f7a485d53
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722424"
---
# <a name="ren"></a>ren

重新命名檔案或目錄。 此命令與**rename**命令相同。



## <a name="syntax"></a>語法

```
ren [<Drive>:][<Path>]<FileName1> <FileName2>
rename [<Drive>:][<Path>]<FileName1> <FileName2>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[\<磁片磁碟機>：][\<路徑>]\<FileName1>|指定要重新命名之檔案或檔案集的位置和名稱。 *FileName1*可以包含萬用字元（**&#42;** 和 **？**）。|
|\<FileName2>|指定檔案的新名稱。 您可以使用萬用字元來指定多個檔案的新名稱。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

- 您無法在重新命名檔案時指定新的磁片磁碟機或路徑。
- 您無法使用**ren**命令來重新命名磁片磁碟機上的檔案，或將檔案移至不同的目錄。
- 您可以在任何一個*FileName*參數中使用萬用字元（**&#42;** 和 **？**）。 *FileName2*中以萬用字元表示的字元將與*FileName1*中的對應字元相同。
- *FileName2*必須是唯一的檔案名。 如果*FileName2*符合現有的檔案名， **ren**會顯示下列訊息：  
  ```
  Duplicate file name or file not found
  ```

## <a name="examples"></a><a name="BKMK_examples"></a>範例

若要將目前目錄中的所有 .txt 副檔名變更為 .doc 副檔名，請輸入：
```
ren *.txt *.doc 
```
若要將目錄的名稱從 Chap10 變更為 Part10，請輸入：
```
ren chap10 part10 
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
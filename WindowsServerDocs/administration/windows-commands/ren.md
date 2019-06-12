---
title: ren
description: 了解如何重新命名檔案或目錄 ren 命令。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60398e12-a05d-4524-a73a-0a925943e21d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 34c761cb08916d277f8f7f1c58d57a05ed2c8daf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441816"
---
# <a name="ren"></a>ren

重新命名檔案或目錄。 此命令等同於**重新命名**命令。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
ren [<Drive>:][<Path>]<FileName1> <FileName2>
rename [<Drive>:][<Path>]<FileName1> <FileName2>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[\<Drive>:][\<Path>]\<FileName1>|指定的位置和檔案名稱或一組您想要重新命名的檔案。 *FileName1*可以包含萬用字元 ( **&#42;** 並 **？** )。|
|\<FileName2>|指定檔案的新名稱。 您可以使用萬用字元來指定多個檔案的新名稱。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

- 重新命名檔案時，您無法指定新的磁碟機或路徑。
- 您無法使用**ren**命令，在磁碟機重新命名檔案，或將檔案移至不同的目錄。
- 您可以使用萬用字元 ( **&#42;** 並 **？** ) 中*FileName*參數。 表示中的萬用字元的字元*FileName2*將會在對應的字元完全相同*FileName1*。
- *FileName2*必須是唯一的檔案名稱。 如果*FileName2*符合現有的檔案名稱， **ren**顯示下列訊息：  
  ```
  Duplicate file name or file not found
  ```

## <a name="BKMK_examples"></a>範例

若要變更所有.txt 檔案的副檔名為.doc 的擴充功能，目前的目錄中輸入：
```
ren *.txt *.doc 
```
若要從 Chap10 Part10 變更目錄的名稱，請輸入：
```
ren chap10 part10 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
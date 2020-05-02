---
title: 標籤
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bbae8bdd-97d4-4566-9118-7c95aa07645f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63603eda8d23b6f7b89b8d1ba858575a60e3c65c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724510"
---
# <a name="label"></a>標籤



建立、變更或刪除磁片的磁片區標籤（也就是名稱）。 如果使用時不含參數， **label**命令會變更目前的磁片區標籤，或刪除現有的標籤。



## <a name="syntax"></a>語法

```
label [/mp] [<Volume>] [<Label>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/mp|指定應該將磁片區視為掛接點或磁片區名稱。|
|\<磁片區>|指定磁碟機號（後面接著冒號）、掛接點或磁片區名稱。 如果指定了磁片區名稱， **/mp**參數就是不必要的。|
|\<標籤>|指定磁片區的標籤。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

- Windows 會顯示磁片區標籤和序號（如果有的話）做為目錄清單的一部分。
- NTFS 磁片區標籤的長度最多可達32個字元，包括空格。 NTFS 磁片區標籤會保留，並顯示建立標籤時所使用的大小寫。
- 如果您未指定**label**參數的值， **label**命令會以下列格式顯示輸出：  
  ```
  Volume in drive C: xxxxxxxxxxx 
  Volume Serial Number is xxxx-xxxx 
  Volume label (32 characters, ENTER for none)?
  ```  
  您可以輸入新的磁片區標籤，或按 ENTER 來保留目前的標籤。 如果您按下 ENTER，而磁片區目前有標籤，則**label**命令會提示您輸入下列訊息：  
  ```
  Delete current volume label (Y/N)?
  ```  
  按 Y 刪除標籤，或按 N 以保留標籤。

## <a name="examples"></a>範例

若要將磁片磁碟機 A 中包含七月銷售資訊的磁片加上標籤，請輸入：
```
label a:sales-july
```
若要刪除磁片磁碟機 C 的目前標籤，請遵循下列步驟：
1. 在命令提示字元中，輸入：  
   ```
   Label
   ```  
   應該會顯示類似下列的輸出：  
   ```
   Volume in drive C: is Main Disk
   Volume Serial Number is 6789-ABCD
   Volume label (32 characters, ENTER for none)?
   ```  
2. 按 ENTER 鍵。 應該會顯示下列提示：  
   ```
   Delete current volume label (Y/N)?
   ```  
3. 按 Y 以刪除目前的標籤。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
---
title: 標籤
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bbae8bdd-97d4-4566-9118-7c95aa07645f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0c68fbbf3ea776bbf6cd49fc4fa446d5dd46542
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437917"
---
# <a name="label"></a>標籤



建立、 變更或刪除磁碟的磁碟區標籤 （也就是名稱）。 如果未指定參數，使用**標籤**命令變更目前的磁碟區標籤，或刪除現有的標籤。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
label [/mp] [<Volume>] [<Label>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/mp|指定磁碟區應該視為掛接點或磁碟區名稱。|
|\<磁碟區 >|指定磁碟機代號 （後面接著冒號），掛接點或磁碟區名稱。 如果指定的磁碟區名稱，則 **/mp**不是必要參數。|
|\<Label>|指定磁碟區標籤。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

- Windows 會顯示磁碟區標籤和序號 （如果有的話） 的目錄清單的一部分。
- NTFS 磁碟區標籤可以是最多 32 個字元長度，包括空格。 NTFS 磁碟區標籤會保留，並顯示已建立標籤時所使用的情況。
- 如果您未指定的值**標籤**參數**標籤**命令會以下列格式顯示輸出：  
  ```
  Volume in drive C: xxxxxxxxxxx 
  Volume Serial Number is xxxx-xxxx 
  Volume label (32 characters, ENTER for none)?
  ```  
  您可以輸入新的磁碟區標籤，或按下 ENTER，以保留目前的標籤。 如果您按 ENTER 鍵，而磁碟區目前有標籤**標籤**命令會提示您使用下列訊息：  
  ```
  Delete current volume label (Y/N)?
  ```  
  按下 Y 鍵來刪除標籤，或按 N 来保留標籤。

## <a name="BKMK_examples"></a>範例

若要標記包含年 7 月的銷售資訊的磁碟機 A 中的磁碟，請輸入：
```
label a:sales-july
```
若要刪除目前的磁碟機 C 的標籤，請遵循下列步驟：
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
3. 按下 Y 鍵來刪除目前的標籤。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
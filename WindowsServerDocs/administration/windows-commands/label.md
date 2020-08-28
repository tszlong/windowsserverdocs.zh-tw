---
title: label
description: 標籤命令的參考文章，可建立、變更或刪除磁片區標籤 (也就是磁片的名稱) 。
ms.topic: reference
ms.assetid: bbae8bdd-97d4-4566-9118-7c95aa07645f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 486461059e90d0d1e1c6fa413e6db595f82924bb
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028196"
---
# <a name="label"></a>label

建立、變更或刪除磁片區標籤 (也就是磁片的名稱) 。 如果未使用參數，卷 **標** 命令會變更目前的磁片區標籤，或刪除現有的標籤。

## <a name="syntax"></a>語法

```
label [/mp] [<volume>] [<label>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /mp | 指定應將磁片區視為掛接點或磁片區名稱。 |
| `<volume>` | 指定磁碟機號 (後面接著冒號) 、掛接點或磁片區名稱。 如果指定了磁片區名稱，則不需要 **/mp** 參數。 |
| `<label>` | 指定磁片區的標籤。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

- Windows 會顯示磁片區標籤和序號 (如果它有一個) 作為目錄清單的一部分。

- NTFS 磁片區標籤的長度最多可達32個字元，包括空格。 NTFS 磁片區標籤會保留並顯示建立標籤時所使用的大小寫。

## <a name="examples"></a>範例

若要為磁片磁碟機 A 中包含七月銷售資訊的磁片加上標籤，請輸入：

```
label a:sales-july
```

若要查看並刪除磁片磁碟機 C 的目前標籤，請遵循下列步驟：

1. 在命令提示字元中，輸入：

   ```
   label
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

3. 按 **Y** 刪除目前的標籤，如果您想要保留現有的標籤，則為 **N** 。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
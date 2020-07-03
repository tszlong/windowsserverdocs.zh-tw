---
title: 標籤
description: 標籤命令的參考文章，會建立、變更或刪除磁片的磁片區標籤（也就是名稱）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bbae8bdd-97d4-4566-9118-7c95aa07645f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8c13285c5dc5030e96d7d334bb65d15f04dff86
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931814"
---
# <a name="label"></a>標籤

建立、變更或刪除磁片的磁片區標籤（也就是名稱）。 如果使用時不含參數， **label**命令會變更目前的磁片區標籤，或刪除現有的標籤。

## <a name="syntax"></a>語法

```
label [/mp] [<volume>] [<label>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| /mp | 指定應該將磁片區視為掛接點或磁片區名稱。 |
| `<volume>` | 指定磁碟機號（後面接著冒號）、掛接點或磁片區名稱。 如果指定了磁片區名稱， **/mp**參數就是不必要的。 |
| `<label>` | 指定磁片區的標籤。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

- Windows 會顯示磁片區標籤和序號（如果有的話）做為目錄清單的一部分。

- NTFS 磁片區標籤的長度最多可達32個字元，包括空格。 NTFS 磁片區標籤會保留，並顯示建立標籤時所使用的大小寫。

## <a name="examples"></a>範例

若要將磁片磁碟機 A 中包含七月銷售資訊的磁片加上標籤，請輸入：

```
label a:sales-july
```

若要查看及刪除 C 磁片磁碟機的目前標籤，請遵循下列步驟：

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

3. 按**Y**以刪除目前的標籤，如果您想要保留現有的標籤，則按**N** 。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
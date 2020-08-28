---
title: 移動
description: 移動命令的參考文章，可將一或多個檔案從一個目錄移至另一個目錄。
ms.topic: reference
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d2bfef9099a2ef590f94dd4effbd011bbc424a5d
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035316"
---
# <a name="move"></a>移動

將一或多個檔案從一個目錄移至另一個目錄。

> [!IMPORTANT]
> 將加密的檔案移至不支援加密檔案系統 (EFS) 結果的磁片區，將會導致錯誤。 您必須先解密檔案，或將檔案移至支援 EFS 的磁片區。

## <a name="syntax"></a>語法

```
move [{/y|-y}] [<source>] [<target>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /y | 停止提示確認是否要覆寫現有的目的地檔案。 此參數在 COPYCMD 環境變數中可能是預設值。 您可以使用 **-y** 參數覆寫這個預設值。 除非從批次腳本內執行命令，否則預設值為在覆寫檔案之前提示。 |
| -y | 開始提示確認您要覆寫現有的目的地檔案。 |
| `<source>` | 指定要移動)  (的路徑和檔案名。 若要移動或重新命名目錄， *來源* 應該是目前的目錄路徑和名稱。 |
| `<target>` | 指定要將檔案移至其中的路徑和名稱。 若要移動或重新命名目錄， *目標* 應該是所需的目錄路徑和名稱。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要將副檔名為 .xls 的所有檔案從 *\Data* 目錄移至 *\ Second_Q \reports* 目錄，請輸入：

```
move \data\*.xls \second_q\reports\
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

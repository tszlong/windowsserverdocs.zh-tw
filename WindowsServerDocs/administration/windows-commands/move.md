---
title: 移動
description: Move 命令的參考文章，可將一個或多個檔案從一個目錄移至另一個目錄。
ms.topic: article
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ceeced7e734775138cc47cba9d36981a4433750
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886324"
---
# <a name="move"></a>移動

將一個或多個檔案從一個目錄移至另一個目錄。

> [!IMPORTANT]
> 將加密的檔案移至不支援加密檔案系統 (EFS) 結果將導致錯誤的磁片區。 您必須先將檔案解密，或將它們移至支援 EFS 的磁片區。

## <a name="syntax"></a>語法

```
move [{/y|-y}] [<source>] [<target>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /y | 停止提示確認您想要覆寫現有的目的地檔案。 在 COPYCMD 環境變數中，此參數可能會預設為預設值。 您可以使用 **-y**參數來覆寫此預設值。 預設值是在覆寫檔案之前先提示，除非命令是從批次腳本中執行。 |
| -y | 開始提示確認您想要覆寫現有的目的檔案。 |
| `<source>` | 指定要移動之檔案的路徑和名稱 (s) 。 若要移動或重新命名目錄，*來源*應該是目前的目錄路徑和名稱。 |
| `<target>` | 指定要用來移動檔案的路徑和名稱。 若要移動或重新命名目錄，*目標*應該是所需的目錄路徑和名稱。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要將*\Data*目錄中副檔名為 .xls 的所有檔案移到*\ Second_Q \reports*目錄，請輸入：

```
move \data\*.xls \second_q\reports\
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

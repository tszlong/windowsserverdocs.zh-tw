---
title: recover
description: 復原命令的參考文章，可從損壞或損毀的磁片復原可讀取的資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab7f502b046bf30a40b1fdd386c7faddc5c8f15a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931932"
---
# <a name="recover"></a>recover

從損壞或損壞的磁片復原可讀取的資訊。 此命令會逐磁區讀取檔案，並從良好的磁區復原資料。 損毀的磁區中的資料會遺失。 因為當您復原檔案時，錯誤磁區中的所有資料都會遺失，所以您應該一次只復原一個檔案。

當您的磁片已準備好進行作業時， **chkdsk**命令回報的錯誤磁區會標示為「錯誤」。 它們不會造成任何危險，而且**復原**也不會影響它們。

## <a name="syntax"></a>語法

```
recover [<drive>:][<path>]<filename>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| `[<drive>:][<path>]<filename>` | 指定您想要復原之檔案的檔案名（如果檔案不在目前目錄中，則為該檔案的位置）。 *Filename*是必要的，而且不支援萬用字元。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要復原磁片磁碟機 D 上*\fiction*目錄中的檔案*story.txt* ，請輸入：

```
recover d:\fiction\story.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

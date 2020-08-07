---
title: recover
description: 復原命令的參考文章，可從損壞或損毀的磁片復原可讀取的資訊。
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0c3d709d76743df4c1a653f0f0a19e8319b0e0f1
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884312"
---
# <a name="recover"></a>recover

從損壞或損壞的磁片復原可讀取的資訊。 此命令會逐磁區讀取檔案，並從良好的磁區復原資料。 損毀的磁區中的資料會遺失。 因為當您復原檔案時，錯誤磁區中的所有資料都會遺失，所以您應該一次只復原一個檔案。

當您的磁片已準備好進行作業時， **chkdsk**命令回報的錯誤磁區會標示為「錯誤」。 它們不會造成任何危險，而且**復原**也不會影響它們。

## <a name="syntax"></a>語法

```
recover [<drive>:][<path>]<filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `[<drive>:][<path>]<filename>` | 如果檔案不在您想要復原的目前目錄) ，請指定檔案名 (和檔案的位置。 *Filename*是必要的，而且不支援萬用字元。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要復原磁片磁碟機 D 上*\fiction*目錄中的檔案*story.txt* ，請輸入：

```
recover d:\fiction\story.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

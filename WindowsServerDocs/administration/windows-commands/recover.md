---
title: recover
description: 復原命令的參考文章，可從損壞或損壞的磁片復原可讀取的資訊。
ms.topic: reference
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad9d2f665bccca1413830e29c082c05a37d93a26
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037136"
---
# <a name="recover"></a>recover

從損壞或損壞的磁片復原可讀取的資訊。 此命令會讀取每個磁區的檔案，並從良好的磁區復原資料。 損毀的磁區中的資料會遺失。 因為當您復原檔案時，不正確的磁區中的所有資料都會遺失，因此您一次只能復原一個檔案。

當您的磁片已準備好進行操作時， **chkdsk** 命令回報的錯誤磁區標示為錯誤。 它們不會造成任何危險，而 **復原** 也不會影響它們。

## <a name="syntax"></a>語法

```
recover [<drive>:][<path>]<filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `[<drive>:][<path>]<filename>` | 指定檔案名 (以及檔案的位置（如果不在您想要復原的目前的目錄) ）。 *檔案名* 是必要的，且不支援萬用字元。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要復原磁片磁碟機 D 上*\fiction*目錄中的檔案*story.txt* ，請輸入：

```
recover d:\fiction\story.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

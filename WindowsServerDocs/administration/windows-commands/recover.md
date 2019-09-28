---
title: recover
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 415efe2d1e60ca70d68059b5702108440da735f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371759"
---
# <a name="recover"></a>recover



從損壞或損壞的磁片復原可讀取的資訊。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
recover [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>參數

|           參數           |                                          描述                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| [\<Drive >：][<Path>] <FileName> | 指定您要復原之檔案的位置和名稱。 需要*FileName* 。 |
|              /?               |                             在命令提示字元顯示說明。                              |

## <a name="remarks"></a>備註

-   **Recover**命令會逐磁區讀取檔案，並從良好的磁區復原資料。 損毀的磁區中的資料會遺失。
-   當您的磁片已準備好進行作業時， **chkdsk**回報的錯誤磁區會標示為「不正確」。 它們不會造成任何危險，而且**復原**也不會影響它們。
-   因為當您復原檔案時，錯誤磁區中的所有資料都會遺失，所以您應該一次只復原一個檔案。
-   您不能使用萬用字元（ **&#42;** 和 **？** ）搭配**recover**命令。 您必須指定檔案（如果檔案不在目前目錄中，則為檔案的位置）。

## <a name="BKMK_examples"></a>典型

若要在磁片磁碟機 D 的 \Fiction 目錄中復原檔案故事 .txt，請輸入：
```
recover d:\fiction\story.txt 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
---
title: recover
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 172471c5c56823e29dd0882072920db3d77639da
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836721"
---
# <a name="recover"></a>recover



從損壞或損壞的磁片復原可讀取的資訊。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
recover [<Drive>:][<Path>]<FileName>
```

### <a name="parameters"></a>參數

|           參數           |                                          描述                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| [\<磁片磁碟機 >：][<Path>]<FileName> | 指定您要復原之檔案的位置和名稱。 需要*FileName* 。 |
|              /?               |                             在命令提示字元顯示說明。                              |

## <a name="remarks"></a>備註

-   **Recover**命令會逐磁區讀取檔案，並從良好的磁區復原資料。 損毀的磁區中的資料會遺失。
-   當您的磁片已準備好進行作業時， **chkdsk**所報告的錯誤磁區會標示為「錯誤」。 它們不會造成任何危險，而且**復原**也不會影響它們。
-   因為當您復原檔案時，錯誤磁區中的所有資料都會遺失，所以您應該一次只復原一個檔案。
-   您不能使用萬用字元（ **&#42;** 和 **？** ）搭配**recover**命令。 您必須指定檔案（如果檔案不在目前目錄中，則為檔案的位置）。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要在磁片磁碟機 D 的 \Fiction 目錄中復原檔案故事 .txt，請輸入：
```
recover d:\fiction\story.txt 
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
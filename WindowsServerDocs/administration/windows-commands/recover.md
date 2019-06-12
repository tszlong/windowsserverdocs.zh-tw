---
title: recover
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d6b9b5544394bfc69a2dc9f7be26ed8355a3f690
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441966"
---
# <a name="recover"></a>recover



從損壞的磁碟復原可讀取的資訊。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
recover [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>參數

|           參數           |                                          描述                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| [\<Drive>:][<Path>]<FileName> | 指定您想要復原的檔案名稱與位置。 *檔名*需要。 |
|              /?               |                             在命令提示字元顯示說明。                              |

## <a name="remarks"></a>備註

-   **復原**命令會讀取檔案時，由磁磁區，並從良好的磁區復原資料。 損壞的磁區中的資料都會遺失。
-   損壞的磁區 reportovanou **chkdsk**標示為 「 不良的 」 磁碟準備作業時。 他們會帶來任何危險，並**復原**並不會影響它們。
-   由於損壞的磁區中的所有資料遺失都時復原檔案時，您應該復原只能有一個檔案一次。
-   您不能使用萬用字元 ( **&#42;** 並 **？** ) 與**復原**命令。 您必須指定的檔案 （和檔案，如果它不是目前的目錄中的位置）。

## <a name="BKMK_examples"></a>範例

若要復原 Story.txt 目錄中的檔案 \Fiction D 磁碟機上，輸入：
```
recover d:\fiction\story.txt 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
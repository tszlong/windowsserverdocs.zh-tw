---
title: 載入中繼資料
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 025f75743d61889c4b987e9a2a575d1c599f04c1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374617"
---
# <a name="load-metadata"></a>載入中繼資料



在匯入可轉移的陰影複製之前載入中繼資料 .cab 檔案，或在還原時載入寫入器中繼資料。 如果使用時不含參數，則**載入中繼資料**會在命令提示字元中顯示說明。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
load metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[\<Drive >：][<Path>]|指定中繼資料檔案的位置。|
|中繼資料 .cab|指定要載入的中繼資料 .cab 檔案。|

## <a name="remarks"></a>備註

-   您可以使用 [匯**入**] 命令，根據**載入中繼資料**所指定的中繼資料，匯入可轉移的陰影複製。
-   在 [**開始還原**] 命令載入所選取的寫入器和要進行還原的元件之前，需要此命令。

## <a name="BKMK_examples"></a>典型

若要從預設位置載入名為中繼檔的中繼資料檔案，請輸入：
```
load metadata metafile.cab
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
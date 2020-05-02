---
title: 載入中繼資料
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d3132bca86533ec3f2d0a27247bd3c116cf55b6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724460"
---
# <a name="load-metadata"></a>載入中繼資料



在匯入可轉移的陰影複製之前載入中繼資料 .cab 檔案，或在還原時載入寫入器中繼資料。 如果使用時不含參數，則**載入中繼資料**會在命令提示字元中顯示說明。



## <a name="syntax"></a>語法

```
load metadata [<Drive>:][<Path>]<MetaData.cab>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[\<磁片磁碟機>：][<Path>]|指定中繼資料檔案的位置。|
|中繼資料 .cab|指定要載入的中繼資料 .cab 檔案。|

## <a name="remarks"></a>備註

-   您可以使用 [匯**入**] 命令，根據**載入中繼資料**所指定的中繼資料，匯入可轉移的陰影複製。
-   在 [**開始還原**] 命令載入所選取的寫入器和要進行還原的元件之前，需要此命令。

## <a name="examples"></a>範例

若要從預設位置載入名為中繼檔的中繼資料檔案，請輸入：
```
load metadata metafile.cab
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
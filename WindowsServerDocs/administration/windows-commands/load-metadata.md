---
title: 載入中繼資料
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b52b5040fc8c834b04cad83ca4b0cfab103fdc43
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871329"
---
# <a name="load-metadata"></a>載入中繼資料



載入中繼資料.cab 檔匯入傳送陰影複製之前，或載入在還原時的寫入器中繼資料。 如果未指定參數，使用**載入中繼資料**在命令提示字元中顯示說明。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
load metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[\<Drive>:][<Path>]|指定之中繼資料檔案的位置。|
|MetaData.cab|指定要載入之中繼資料.cab 檔案。|

## <a name="remarks"></a>備註

-   您可以使用**匯入**命令以匯入傳送陰影複製會根據所指定的中繼資料**載入中繼資料**。
-   此命令需要再**開始還原**命令以載入所選的寫入器和還原元件。

## <a name="BKMK_examples"></a>範例

若要載入的預設位置從呼叫 metafile.cab 中繼資料檔案，請輸入：
```
load metadata metafile.cab
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
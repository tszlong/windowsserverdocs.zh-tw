---
title: set_2
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3f5523646fddbfec31cb3900fc09230efc1c7813
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862609"
---
# <a name="set2"></a>set_2



設定內容、 選項、 詳細資訊模式，以及建立陰影複製的中繼資料檔案。 如果未指定參數，使用**設定**列出所有目前的設定。

## <a name="syntax"></a>語法

```
set
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
set verbose {on|off}
set metadata <MetaData.cab>
```

## <a name="set-sub-commands"></a>設定子命令

|子命令|描述|
|-----------|-----------|
|內容|設定建立陰影複製的內容。 請參閱[設定內容](set-context.md)語法和參數。|
|選項|設定選項建立陰影複製。 請參閱[設定選項](set-option.md)語法和參數。|
|詳細資訊|開啟或關閉，請開啟詳細資訊輸出模式。 請參閱[設定 verbose](set-verbose.md)語法和參數。|
|中繼資料|設定名稱和陰影建立中繼資料檔案的位置。 請參閱[集中繼資料](set-metadata.md)語法和參數。|
|/?|在命令提示字元顯示說明。|

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
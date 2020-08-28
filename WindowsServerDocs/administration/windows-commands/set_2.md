---
title: set_2
description: Set_2 的參考文章，可設定用於建立陰影複製的內容、選項、詳細資訊模式和中繼資料檔案。
ms.topic: reference
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b2751b7fd5d550f9499f12acde6f5f7e993d095
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023412"
---
# <a name="set_2"></a>set_2

設定用於建立陰影複製的內容、選項、詳細資訊模式和中繼資料檔案。 如果使用時不含參數， **set** 會列出所有目前的設定。

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
|內容|設定用於建立陰影複製的內容。 請參閱設定語法和參數的 [內容](set-context.md) 。|
|選項|設定用於建立陰影複製的選項。 請參閱語法和參數的 [Set 選項](set-option.md) 。|
|verbose|開啟或關閉詳細資訊輸出模式。 請參閱設定語法和參數的 [詳細](set-verbose.md) 資訊。|
|中繼資料|設定陰影建立中繼資料檔案的名稱和位置。 請參閱設定語法和參數的 [中繼資料](set-metadata.md) 。|
|/?|在命令提示字元顯示說明。|

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
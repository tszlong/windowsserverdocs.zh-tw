---
title: set_2
description: 適用于 set_2 的 Windows 命令主題，用於設定陰影複製建立的內容、選項、詳細資訊模式和中繼資料檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fa467625997824a11b2303572a063d591f59bdd6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834381"
---
# <a name="set_2"></a>set_2

設定陰影複製建立的內容、選項、詳細資訊模式和中繼資料檔案。 如果使用時不含參數，則**設定**會列出所有目前的設定。

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
|內容|設定陰影複製建立的內容。 請參閱設定語法和參數的[內容](set-context.md)。|
|替代|設定陰影複製建立的選項。 如需語法和參數，請參閱[Set 選項](set-option.md)。|
|詳細資訊|開啟或關閉詳細資訊輸出模式。 請參閱設定語法和參數的[詳細](set-verbose.md)資訊。|
|中繼資料|設定陰影建立中繼資料檔案的名稱和位置。 請參閱設定語法和參數的[中繼資料](set-metadata.md)。|
|/?|在命令提示字元顯示說明。|

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
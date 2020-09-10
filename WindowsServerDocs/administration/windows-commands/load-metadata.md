---
title: load metadata
description: Load metadata 命令的參考文章，此命令會在匯入可傳送的陰影複製之前載入中繼資料 .cab 檔，或在還原時載入寫入器中繼資料。
ms.topic: reference
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1d2895b4122d54de92dd595c5dc7218b5579acb6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639540"
---
# <a name="load-metadata"></a>載入中繼資料

在匯入可傳送的陰影複製之前載入中繼資料 .cab 檔，或在還原的情況下載入寫入器中繼資料。 如果使用時不含參數，則 **載入中繼資料** 會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
load metadata [<drive>:][<path>]<metadata.cab>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `[<drive>:][<path>]` | 指定中繼資料檔案的位置。 |
| metadata.cab | 指定要載入的中繼資料 .cab 檔。 |

## <a name="remarks"></a>備註

- 您可以使用 [匯 **入** ] 命令，根據 **載入中繼資料**所指定的中繼資料，匯入可轉移的陰影複製。

- 您必須在 **begin restore** 命令之前執行此命令，以載入所選的寫入器和用於還原的元件。

## <a name="examples"></a>範例

若要從預設位置載入名為 metafile.cab 的中繼資料檔案，請輸入：

```
load metadata metafile.cab
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [匯入 diskshadow 命令](import.md)

- [開始還原命令](begin-restore.md)

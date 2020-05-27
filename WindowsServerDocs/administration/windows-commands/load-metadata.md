---
title: 載入中繼資料
description: '[載入中繼資料] 命令的參考主題，它會在匯入可轉移的陰影複製之前載入中繼資料 .cab 檔案，或在還原時載入寫入器中繼資料。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c7dc967476412261e7afc228088566f74ec4208c
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820188"
---
# <a name="load-metadata"></a>載入中繼資料

在匯入可轉移的陰影複製之前載入中繼資料 .cab 檔案，或在還原時載入寫入器中繼資料。 如果使用時不含參數，則**載入中繼資料**會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
load metadata [<drive>:][<path>]<metadata.cab>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `[<drive>:][<path>]` | 指定中繼資料檔案的位置。 |
| 中繼資料 .cab | 指定要載入的中繼資料 .cab 檔案。 |

## <a name="remarks"></a>備註

- 您可以使用 [匯**入**] 命令，根據**載入中繼資料**所指定的中繼資料，匯入可轉移的陰影複製。

- 您必須在 [**開始還原**] 命令之前執行此命令，以載入所選取的寫入器和要進行還原的元件。

## <a name="examples"></a>範例

若要從預設位置載入名為中繼檔的中繼資料檔案，請輸入：

```
load metadata metafile.cab
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [匯入 diskshadow 命令](import.md)

- [開始還原命令](begin-restore.md)

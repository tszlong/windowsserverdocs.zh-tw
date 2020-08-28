---
title: 設定中繼資料
description: 設定中繼資料的參考文章，可設定陰影建立中繼資料檔案的名稱和位置，該檔案是用來將陰影複製從一部電腦傳送到另一部電腦。
ms.topic: reference
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dabec963b84a5c8d5acfd5e214b8d62d4740d9b3
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024942"
---
# <a name="set-metadata"></a>設定中繼資料

設定用來將陰影複製從一部電腦傳送到另一部電腦的陰影建立中繼資料檔案的名稱和位置。 如果使用時不含參數， **設定中繼資料** 會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
set metadata [<Drive>:][<Path>]<MetaData.cab>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[\<Drive>:][<Path>]|指定要建立中繼資料檔案的位置。|
|\<MetaData.cab>|指定要儲存陰影建立中繼資料的 cab 檔案名。|

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
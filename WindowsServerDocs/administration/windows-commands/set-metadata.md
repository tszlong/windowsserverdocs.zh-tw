---
title: 設定中繼資料
description: 設定中繼資料的參考文章，可設定陰影建立中繼資料檔案的名稱和位置，該檔案是用來將陰影複製從一部電腦傳送到另一部電腦。
ms.topic: reference
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b680ac538f7c2593871137b07d045ef150b68b1f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637679"
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
---
title: 設定中繼資料
description: 設定中繼資料的參考文章，設定用來將陰影複製從一部電腦傳輸到另一部電腦的陰影建立中繼資料檔案的名稱和位置。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50c9ceebf072db2e7cefada1601accc97b5d0f7f
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85937088"
---
# <a name="set-metadata"></a>設定中繼資料

設定陰影建立中繼資料檔案的名稱和位置，用來將陰影複製從一部電腦傳輸到另一部電腦。 如果使用時不含參數，**設定中繼資料**會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
set metadata [<Drive>:][<Path>]<MetaData.cab>
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|[\<Drive>:][<Path>]|指定要建立中繼資料檔案的位置。|
|\<MetaData.cab>|指定要儲存陰影建立中繼資料的 cab 檔案名稱。|

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
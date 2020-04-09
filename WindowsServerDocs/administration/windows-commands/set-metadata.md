---
title: 設定中繼資料
description: 適用于設定中繼資料的 Windows 命令主題，其會設定陰影建立中繼資料檔案的名稱和位置，以用來將陰影複製從一部電腦傳輸到另一部電腦。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5bd728650cf163f98a82ff1e6f88755c4cc1aea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834521"
---
# <a name="set-metadata"></a>設定中繼資料

設定陰影建立中繼資料檔案的名稱和位置，用來將陰影複製從一部電腦傳輸到另一部電腦。 如果使用時不含參數，**設定中繼資料**會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
set metadata [<Drive>:][<Path>]<MetaData.cab>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[\<磁片磁碟機 >：][<Path>]|指定要建立中繼資料檔案的位置。|
|\<的中繼資料 .cab >|指定要儲存陰影建立中繼資料的 cab 檔案名稱。|

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
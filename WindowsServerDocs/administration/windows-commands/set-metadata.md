---
title: 集中繼資料
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82d2cd9ba447a0ea261f91dc01c11e45dfc0aa9b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835989"
---
# <a name="set-metadata"></a>集中繼資料



設定用來將陰影複製從一部電腦傳輸到另一個的陰影建立中繼資料檔案的位置與名稱。 如果未指定參數，使用**集中繼資料**在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
set metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[\<Drive>:][<Path>]|指定要建立的中繼資料檔案的位置。|
|\<MetaData.cab>|指定儲存陰影建立中繼資料的封包檔的名稱。|

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
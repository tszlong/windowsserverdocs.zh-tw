---
title: bitsadmin getcustomheaders
description: '**Bitsadmin getcustomheaders**的 Windows 命令主題-從作業中抓取自訂 HTTP 標頭。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 039669fca42803ff22eb4e3d13dfdef5f0a06f93
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381656"
---
# <a name="bitsadmin-getcustomheaders"></a>bitsadmin getcustomheaders



從作業中抓取自訂 HTTP 標頭。

## <a name="syntax"></a>語法

```
bitsadmin /GetCustomHeaders <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型

下列範例會取得名為*myDownloadJob*之作業的自訂標頭。
```
C:\>bitsadmin /GetCustomHeaders myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
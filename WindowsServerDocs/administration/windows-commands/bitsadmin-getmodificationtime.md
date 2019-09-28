---
title: bitsadmin getmodificationtime
description: '**Bitsadmin getmodificationtime**的 Windows 命令主題-抓取上次修改作業的時間，或成功傳輸資料的時間。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 48b4d252ce6161b288726190f41f08c64efdbcf2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381534"
---
# <a name="bitsadmin-getmodificationtime"></a>bitsadmin getmodificationtime



抓取上次修改作業或資料傳輸成功的時間。

## <a name="syntax"></a>語法

```
bitsadmin /GetModificationTime <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型

下列範例會針對名為*myDownloadJob*的作業，抓取上次修改時間。
```
C:\>bitsadmin /GetModificationTime myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
---
title: bitsadmin getbytestotal
description: '**Bitsadmin getbytestotal**的 Windows 命令主題-抓取指定工作的大小。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38b0f09e13919e0c75d2b7429dd66f765b434de5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381753"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal



抓取指定之作業的大小

## <a name="syntax"></a>語法

```
bitsadmin /GetBytesTotal <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型

下列範例會捕獲名為*myDownloadJob*的作業大小。
```
C:\>bitsadmin /GetBytesTotal myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
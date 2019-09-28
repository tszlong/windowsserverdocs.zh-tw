---
title: bitsadmin getfilestotal
description: '**Bitsadmin getfilestotal**的 Windows 命令主題-抓取指定之作業中的檔案數目。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27cf04e8745aeab5cd1f2ce379c8506be642fea2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381612"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal



抓取指定之作業中的檔案數目。

## <a name="syntax"></a>語法

```
bitsadmin /GetFilesTotal <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*之作業中包含的檔案數目。
```
C:\>bitsadmin /GetFilesTotal myDownloadJob
```

# #

[命令列語法索引鍵](command-line-syntax-key.md)另請參閱
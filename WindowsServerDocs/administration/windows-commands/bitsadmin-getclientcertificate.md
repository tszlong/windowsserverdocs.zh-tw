---
title: bitsadmin getclientcertificate
description: '**Bitsadmin getclientcertificate**的 Windows 命令主題-從作業抓取用戶端憑證。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fc8f408-085e-43a0-9fa8-3d798ef107b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 613feafb442f63513d34e9038647c4dbeb278630
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381723"
---
# <a name="bitsadmin-getclientcertificate"></a>bitsadmin getclientcertificate



從作業抓取用戶端憑證。

## <a name="syntax"></a>語法

```
bitsadmin /GetClientCertificate <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*之作業的用戶端憑證。
```
C:\>bitsadmin / GetClientCertificate myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
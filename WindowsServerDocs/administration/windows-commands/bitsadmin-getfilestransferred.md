---
title: bitsadmin getfilestransferred
description: '**Bitsadmin getfilestransferred**的 Windows 命令主題-抓取所指定工作的已傳輸檔案數目。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e282815c-938b-4ac0-a09d-9baafb656dcb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d02d9d7bc216a5ad7ca922e716c368f64c4b9a44
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381597"
---
# <a name="bitsadmin-getfilestransferred"></a>bitsadmin getfilestransferred



抓取指定作業所傳輸的檔案數目。

## <a name="syntax"></a>語法

```
bitsadmin /GetFilesTransferred <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*的作業中傳送的檔案數目。
```
C:\>bitsadmin /GetFilesTransferred myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
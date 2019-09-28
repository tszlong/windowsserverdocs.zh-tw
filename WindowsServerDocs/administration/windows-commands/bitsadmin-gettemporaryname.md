---
title: bitsadmin gettemporaryname
description: 適用于**bitsadmin gettemporaryname**的 Windows 命令主題-報告作業中指定檔案的暫存檔案案。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b665fae4c0bfdd5ea04b929be49f9590430b358
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381296"
---
# <a name="bitsadmin-gettemporaryname"></a>bitsadmin gettemporaryname



報告作業中指定檔案的暫存檔案案。

## <a name="syntax"></a>語法

```
bitsadmin /GetTemporaryName <Job> <file index> 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|檔案索引|從0開始|

## <a name="BKMK_examples"></a>典型

下列範例會針對名為*myJob*的作業報告檔案2的暫存檔案名。
```
C:\>bitsadmin /GetTemporaryName myJob 1 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
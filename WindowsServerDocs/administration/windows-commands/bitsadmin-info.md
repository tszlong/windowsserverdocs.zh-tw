---
title: bitsadmin info
description: 的 Windows 命令主題會**顯示指定作業的摘要資訊。** -bitsadmin 資訊
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b6710d73860315fcd13670669871cd310ffb41c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381085"
---
# <a name="bitsadmin-info"></a>bitsadmin info



顯示指定之作業的摘要資訊。

## <a name="syntax"></a>語法

```
bitsadmin /Info <Job> [/verbose]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

使用/verbose 參數來提供作業的詳細資訊。

## <a name="BKMK_examples"></a>典型

下列範例會捕獲名為*myDownloadJob*之作業的相關資訊。
```
C:\>bitsadmin /Info myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
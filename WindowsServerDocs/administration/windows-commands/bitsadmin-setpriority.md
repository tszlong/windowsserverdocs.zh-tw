---
title: bitsadmin setpriority
description: '**Bitsadmin setpriority**的 Windows 命令主題-設定指定之作業的優先順序。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60564350928f917ca1861684e042304d5d380426
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380444"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority



設定指定之作業的優先順序。

## <a name="syntax"></a>語法

```
bitsadmin /SetPriority <Job> <Priority>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|Priority|下列其中一個值：</br>-前景</br>-高</br>-正常</br>-低|

## <a name="BKMK_examples"></a>典型

下列範例會將名為*myDownloadJob*之作業的優先順序設定為 normal。
```
C:\>bitsadmin /SetPriority myDownloadJob NORMAL
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
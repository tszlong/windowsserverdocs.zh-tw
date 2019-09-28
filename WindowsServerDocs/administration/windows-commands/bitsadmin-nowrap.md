---
title: bitsadmin nowrap
description: 適用于**bitsadmin nowrap**的 Windows 命令主題-截斷任何延伸到命令視窗最右邊邊緣之外的輸出文字行。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3806ec51161eeae498e3c9b367b2aacf0bd32c99
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381046"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

截斷任何延伸到命令視窗最右邊邊緣外的輸出文字行。

## <a name="syntax"></a>語法

```
bitsadmin /NoWrap
```

## <a name="remarks"></a>備註

根據預設，除了**監視器**參數之外，所有參數都會包裝輸出。 在其他參數前指定**NoWrap**參數。

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*之作業的狀態，而且不會包裝輸出
```
C:\>bitsadmin /NoWrap /GetState myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
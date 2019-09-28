---
title: bitsadmin util 和版本
description: 適用于**bitsadmin util 和版本**的 Windows 命令主題-顯示 BITS 服務的版本。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 495ef17bbf6f39f20f6729b64de4b4bec0f9a3c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380205"
---
# <a name="bitsadmin-util-and-version"></a>bitsadmin util 和版本

顯示 BITS 服務的版本（例如，2.0）。

**BITSAdmin 1.5 和更早版本**： 未支援。

## <a name="syntax"></a>語法

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>備註

**Verbose**參數會執行下列動作：
-   顯示每個位相關 DLL 的檔案版本
-   確認可以啟動 BITS 服務
-   顯示位群組原則值（僅限 Windows Vista）

## <a name="BKMK_examples"></a>典型

下列範例是 BITS 服務的版本。
```
C:\>bitsadmin /Util /Version
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
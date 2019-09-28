---
title: ksetup： dumpstate
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 625d05b2fea9ae58681648c64e309aa8b2a201ed
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375005"
---
# <a name="ksetupdumpstate"></a>ksetup： dumpstate



顯示電腦上定義之所有領域的領域設定目前狀態。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /dumpstate
```

### <a name="parameters"></a>參數

None

## <a name="remarks"></a>備註

此命令的輸出包含預設領域（電腦所屬的網域），以及此電腦上定義的所有領域。 每個領域包含下列各項：
-   與此領域相關聯的所有金鑰發佈中心（Kdc）
-   此領域的所有**設定領域**旗標
-   KDC 密碼

此命令不會顯示 DNS 偵測或命令**ksetup/domain**所指定的功能變數名稱。

此命令不會顯示使用 [ **ksetup/setcomputerpassword**] 命令所設定的電腦密碼。

**Ksetup**會產生與**Ksetup/dumpstate**相同的輸出。

## <a name="BKMK_Examples"></a>典型

在電腦上尋找大部分的 Kerberos 領域設定：
```
ksetup /dumpstate
```

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   [命令列語法關鍵](command-line-syntax-key.md)
---
title: ksetup:dumpstate
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8e5e8f20188fc27cc08dfd37c5fdbd811925f476
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863119"
---
# <a name="ksetupdumpstate"></a>ksetup:dumpstate



顯示目前狀態的電腦所定義的所有領域的領域設定。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /dumpstate
```

### <a name="parameters"></a>參數

None

## <a name="remarks"></a>備註

這個命令的輸出會包含預設領域 （電腦所隸屬的網域） 和此電腦所定義的所有領域。 以下是包含每個領域：
-   所有金鑰發佈中心 (Kdc) 與此領域相關聯
-   所有**組領域**此領域的旗標
-   KDC 密碼

此命令不會顯示 DNS 偵測或命令所指定的網域名稱**ksetup /domain**。

此命令不會顯示透過使用命令的電腦密碼**ksetup /setcomputerpassword**。

**Ksetup**會產生相同的輸出**ksetup /dumpstate**。

## <a name="BKMK_Examples"></a>範例

在電腦上，尋找 Kerberos 領域組態的大部分：
```
ksetup /dumpstate
```

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   [命令列語法關鍵](command-line-syntax-key.md)
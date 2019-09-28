---
title: active
description: 適用于**active** basic 磁片的 Windows 命令主題，會將具有焦點的磁碟分割標示為作用中。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c926bf9b7a583cf7eaa23166e09e6f0a1599e625
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382855"
---
# <a name="active"></a>active



在 [基本磁碟] 上，將具有焦點的磁碟分割標示為作用中。

> [!CAUTION]
> DiskPart 只會驗證磁碟分割是否能夠包含作業系統啟動檔案。 DiskPart 不會檢查磁碟分割的內容。 如果您不小心將磁碟分割標示為使用中，而且它不包含作業系統啟動檔案，則您的電腦可能無法啟動。

## <a name="syntax"></a>語法

```
active
```

## <a name="remarks"></a>備註

-   這會通知基本輸入/輸出系統（BIOS）或可延伸固件介面（EFI），磁碟分割或磁片區是有效的系統磁碟分割或系統磁碟區。
-   只有資料分割可以標示為使用中。
-   必須選取分割區，此作業才會成功。 使用 [**選取資料分割**] 命令來選取磁碟分割，並將焦點移至該資料分割。

## <a name="BKMK_examples"></a>典型

若要將具有焦點的資料分割標示為使用中的資料分割，請輸入：
```
active
```

#### <a name="additional-references"></a>其他參考資料


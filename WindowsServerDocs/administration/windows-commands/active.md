---
title: active
description: 適用於 Windows 命令主題**active** -基本磁碟上的標記具有為作用中焦點的磁碟分割。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3a039e0200fb84d446739ac7017556b6c302f4af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868759"
---
# <a name="active"></a>active



基本磁碟上的標記具有為作用中焦點的磁碟分割。

> [!CAUTION]
> DiskPart 只確認資料分割是包含作業系統啟動檔案。 DiskPart 不會檢查分割區的內容。 如果您不小心將標示為作用中的資料分割，而且它不包含作業系統啟動檔案，您的電腦可能無法啟動。

## <a name="syntax"></a>語法

```
active
```

## <a name="remarks"></a>備註

-   這會通知的基本輸入/輸出系統 (BIOS) 或可延伸韌體介面 (EFI) 的磁碟分割或磁碟區是有效的系統磁碟分割或系統磁碟區。
-   只有資料分割可以標示為作用中。
-   這項作業成功時，必須選取資料分割。 使用**選取資料分割**命令來選取資料分割，並將焦點移到它。

## <a name="BKMK_examples"></a>範例

若要在標記中的磁碟分割時的焦點的磁碟分割，請輸入：
```
active
```

#### <a name="additional-references"></a>其他參考資料


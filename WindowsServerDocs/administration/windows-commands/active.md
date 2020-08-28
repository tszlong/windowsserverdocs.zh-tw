---
title: 作用中
description: 作用中命令的參考文章（在基本磁碟上）會將具有焦點的磁碟分割標記為作用中。
ms.topic: reference
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6baad8bd60307eeddf7b777f059ab5ba3846e1d4
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029475"
---
# <a name="active"></a>作用中

在基本磁碟上，將帶有焦點的磁碟分割標記為啟動。 只有資料分割可以標示為使用中。 必須選取一個磁碟分割，這項操作才能繼續。 使用 [ **選取資料分割** ] 命令來選取分割區，並將焦點移至該分割區。

> [!CAUTION]
> DiskPart 只會通知基本的輸入/輸出系統 (BIOS) 或可延伸的固件介面 (EFI) 磁碟分割或磁片區是有效的系統磁碟分割或系統磁碟區，而且能夠包含作業系統啟動檔案。 DiskPart 不檢查磁碟分割的內容。 如果您誤將磁碟分割標記為作用中，但它並未包含作業系統啟動檔案，則您的電腦可能無法啟動。

## <a name="syntax"></a>語法

```
active
```

## <a name="examples"></a>範例

若要將焦點標示為使用中磁碟分割的資料分割，請輸入：

```
active
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [選取資料分割命令](select-partition.md)

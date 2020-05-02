---
title: 作用中
description: 作用中命令的參考主題（在基本磁碟上）會將具有焦點的磁碟分割標示為作用中。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 997c57b93434738c87396812c9b5e5b12d7a8e89
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719020"
---
# <a name="active"></a>作用中

在基本磁碟上，將帶有焦點的磁碟分割標記為啟動。 只有資料分割可以標示為使用中。 必須選取一個磁碟分割，這項操作才能繼續。 使用 [**選取資料分割**] 命令來選取磁碟分割，並將焦點移至該資料分割。

> [!CAUTION]
> DiskPart 只會通知基本輸入/輸出系統（BIOS）或可延伸韌體介面（EFI），磁碟分割或磁片區是有效的系統磁碟分割或系統磁碟區，而且能夠包含作業系統啟動檔案。 DiskPart 不檢查磁碟分割的內容。 如果您不小心將磁碟分割標示為使用中，而且它不包含作業系統啟動檔案，則您的電腦可能無法啟動。

## <a name="syntax"></a>語法

```
active
```

## <a name="examples"></a>範例

若要將具有焦點的資料分割標示為使用中的資料分割，請輸入：

```
active
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [選取資料分割命令](select-partition.md)

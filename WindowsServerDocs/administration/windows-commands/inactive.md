---
title: 非使用
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ce91b6a024c165e3aa63148b9ad6dfcc4db7a7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375369"
---
# <a name="inactive"></a>非使用



在基本的主開機記錄（MBR）磁片上，將具有焦點的系統磁碟分割或開機磁碟分割標示為非作用中。

## <a name="syntax"></a>語法

```
inactive
```

## <a name="remarks"></a>備註

> [!CAUTION]
> 如果沒有作用中的磁碟分割，您的電腦可能無法啟動。 請不要將系統或開機磁碟分割標記為非使用中，除非您是有經驗的使用者徹底瞭解 Windows 系列的作業系統。</br>> 如果您在將系統或開機磁碟分割標示為非作用中之後無法啟動電腦，請將 Windows 安裝程式 CD 插入 CD-ROM 光碟機中，重新開機電腦，然後使用中的**fixmbr**和**fixboot**命令修復磁碟分割修復主控台。
> -   將系統磁碟分割或開機磁碟分割標記為非使用中之後，您的電腦會從 BIOS 中指定的下一個選項（例如，CD-ROM 光碟機或開機前執行環境（PXE））開始。
> -   必須選取使用中的系統或開機磁碟分割，此操作才能成功。 使用 [**選取資料分割**] 命令來選取使用中的資料分割，並將焦點移至其上。

## <a name="BKMK_examples"></a>典型

```
inactive
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


---
title: 詳細資料磁片
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff78a3f9e27cde35a7e19bdf1565c515a127261b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378582"
---
# <a name="detail-disk"></a>詳細資料磁片



顯示所選磁碟的內容以及該磁碟上的磁碟區。

## <a name="syntax"></a>語法

```
detail disk
```

## <a name="remarks"></a>備註

-   必須選取磁片，此操作才能成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。
-   如果選取的磁片是虛擬硬碟（VHD），**詳細資料磁片**會將磁片的匯流排類型報告為 [虛擬]。

## <a name="BKMK_examples"></a>典型

若要查看所選磁片的內容，以及磁片中磁片區的相關資訊，請輸入：
```
detail disk
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


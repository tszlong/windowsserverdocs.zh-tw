---
title: 詳細資料量
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 38f2bc75-2ed6-4e80-aa74-ab83133db1cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8cd5889bfd2aea835cb64ef1a4076faee0f39b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378474"
---
# <a name="detail-volume"></a>詳細資料量



顯示目前磁片區所在的磁片。

## <a name="syntax"></a>語法

```
detail volume
```

## <a name="remarks"></a>備註

-   必須選取磁片區，此操作才能成功。 使用 [**選取磁片**區] 命令來選取磁片區，並將焦點移至該磁片區。
-   磁片區詳細資料不適用於唯讀磁片區，例如 DVD-ROM 或 CD-ROM 光碟機。

## <a name="BKMK_examples"></a>典型

若要查看目前磁片區所在的所有磁片，請輸入：
```
detail volume
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


---
title: 匯入
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca0a15e73e4aa913ece34e083a8070be3b4b190d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375414"
---
# <a name="import"></a>匯入



將外部磁片群組匯入本機電腦的磁片群組。

## <a name="syntax"></a>語法

```
import [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|

## <a name="remarks"></a>備註

-   Import 命令會匯入與具有焦點的磁片位於相同群組中的每個磁片。
-   必須選取外部磁片群組中的動態磁碟，此操作才會成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。

## <a name="BKMK_examples"></a>典型

若要匯入相同磁片群組中的每個磁片，並將焦點放入本機電腦的磁片群組，請輸入：
```
import
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


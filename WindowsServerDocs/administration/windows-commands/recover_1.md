---
title: recover
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cc3a73d-9456-41a0-b375-2b4cc37c3992
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a83bb7502145cc09116241ea255e31b5f9981791
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384744"
---
# <a name="recover"></a>recover



重新整理磁片群組中所有磁片的狀態、嘗試復原無效磁片群組中的磁片，以及重新同步擁有過時資料的鏡像磁碟區和 RAID-5 磁片區。

> [!IMPORTANT]
> Windows Vista 的任何版本都無法使用此 DiskPart 命令。

## <a name="syntax"></a>語法

```
recover [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|

## <a name="remarks"></a>備註

-   此命令會在磁片群組上操作。
-   此命令只適用于動態磁碟群組。 如果此命令用於具有基本磁碟的群組，則不會傳回錯誤，但不會採取任何動作。
-   此命令會在失敗或失敗的磁片上操作。 它也會在失敗、失敗或處於失敗的冗余狀態的磁片區上運作。
-   必須選取屬於磁片群組一部分的磁片，此命令才會成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。

## <a name="BKMK_examples"></a>典型

若要復原包含具有焦點之磁片的磁片群組，請輸入：
```
recover
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


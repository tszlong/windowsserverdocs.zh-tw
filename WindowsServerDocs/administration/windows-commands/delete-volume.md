---
title: delete volume
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 35b22e1bfc6fbfca8ef7bd29bfe1b7e28d7d35d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378649"
---
# <a name="delete-volume"></a>delete volume



刪除選取的磁碟區。

## <a name="syntax"></a>語法

```
delete volume [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|

## <a name="remarks"></a>備註

-   您無法刪除系統磁碟區、開機磁碟區或任何包含使用中分頁檔或損毀傾印 (記憶體傾印) 的磁碟區。
-   必須選取磁片區，此操作才能成功。 使用 [**選取磁片**區] 命令來選取磁片區，並將焦點移至該磁片區。

## <a name="BKMK_examples"></a>典型

若要刪除具有焦點的磁片區，請輸入：
```
delete volume
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


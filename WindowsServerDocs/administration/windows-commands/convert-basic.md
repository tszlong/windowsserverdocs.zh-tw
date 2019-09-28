---
title: convert basic
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61329896-3b56-4959-8d58-45cbe18ba860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4b81126f4a623d841bb5868f786678d7b093581
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379127"
---
# <a name="convert-basic"></a>convert basic



將空的動態磁碟轉換為基本磁碟。

如需有關如何使用此命令的指示，請參閱[將動態磁碟變更回基本磁碟](https://go.microsoft.com/fwlink/?LinkId=207048)（ https://go.microsoft.com/fwlink/?LinkId=207048) 。

## <a name="syntax"></a>語法

```
convert basic [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|

## <a name="remarks"></a>備註

> [!IMPORTANT]
> 磁片必須是空的，才能將它轉換成基本磁碟。 在轉換磁片之前，請先備份您的資料，然後刪除所有磁碟分割或磁片區。
> -   必須選取動態磁碟，此操作才會成功。 使用 [**選取磁片**] 命令來選取動態磁碟，並將焦點移至其上。

## <a name="BKMK_examples"></a>典型

若要將選取的動態磁碟轉換為基本，請輸入：
```
convert basic
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


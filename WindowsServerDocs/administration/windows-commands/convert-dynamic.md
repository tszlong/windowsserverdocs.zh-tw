---
title: 轉換動態
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15c15d14aeb440c5d7862f0a304f223988f52bbe
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379105"
---
# <a name="convert-dynamic"></a>轉換動態



將基本磁碟轉換成動態磁碟。

如需有關如何使用此命令的指示，請參閱[將基本磁碟變更為動態磁碟](https://go.microsoft.com/fwlink/?LinkId=207047)（ https://go.microsoft.com/fwlink/?LinkId=207047) 。

## <a name="syntax"></a>語法

```
convert dynamic [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|

## <a name="remarks"></a>備註

-   基本磁碟上的任何現有磁碟分割都會變成簡單磁片區。
-   必須選取基本磁碟，此操作才能成功。 使用 [**選取磁片**] 命令來選取基本磁碟，並將焦點轉移到其上。

## <a name="BKMK_examples"></a>典型

若要將基本磁碟轉換成動態磁碟，請輸入：
```
convert dynamic
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


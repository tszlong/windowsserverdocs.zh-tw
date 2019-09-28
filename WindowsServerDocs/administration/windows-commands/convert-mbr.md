---
title: convert mbr
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a635a4c0-af73-4330-b021-51d483424537
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47001415158b3bdb0b06af9114b995a6f0634da1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379068"
---
# <a name="convert-mbr"></a>convert mbr



將具有 GUID 磁碟分割表格（GPT）磁碟分割樣式的空白基本磁碟，轉換為具有主開機記錄（MBR）磁碟分割樣式的基本磁碟。

> [!IMPORTANT]
> 磁片必須是空的，才能將它轉換成 MBR 磁碟。 在轉換磁片之前，請先備份您的資料，然後刪除所有磁碟分割或磁片區。

如需有關如何使用此命令的指示，請參閱[將 GUID 磁碟分割表格磁片變更為主開機記錄磁片](https://go.microsoft.com/fwlink/?LinkId=207050)（ https://go.microsoft.com/fwlink/?LinkId=207050) 。

## <a name="syntax"></a>語法

```
convert mbr [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|

## <a name="remarks"></a>備註

-   必須選取基本磁碟，此操作才能成功。 使用 [**選取磁片**] 命令來選取基本磁碟，並將焦點轉移到其上。

## <a name="BKMK_examples"></a>典型

若要將基本光碟從 GPT 磁碟分割樣式轉換為 MBR 磁碟分區樣式，請輸入 >：
```
convert mbr
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


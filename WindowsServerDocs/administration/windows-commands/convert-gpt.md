---
title: convert gpt
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b3b1b747-0a7a-4be2-8487-2c4be16ee190
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a6392cbcff618c642b9d0f168fe555e8be9e759
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379089"
---
# <a name="convert-gpt"></a>convert gpt



將具有主開機記錄（MBR）磁碟分割樣式的空白基本磁碟轉換成具有 GUID 磁碟分割表格（GPT）磁碟分割樣式的基本磁碟。

如需有關如何使用此命令的指示，請參閱[將主開機記錄磁片變更為 GUID 磁碟分割表格磁片](https://go.microsoft.com/fwlink/?LinkId=207049)（ https://go.microsoft.com/fwlink/?LinkId=207049) 。

## <a name="syntax"></a>語法

```
convert gpt [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|

## <a name="remarks"></a>備註

> [!IMPORTANT]
> 磁片必須是空的，才能將它轉換成 GPT 磁片。 在轉換磁片之前，請先備份您的資料，然後刪除所有磁碟分割或磁片區。
> -   轉換至 GPT 所需的最小磁片大小為 128 mb。
> -   必須選取基本的 MBR 磁碟，此操作才能成功。 使用 [**選取磁片**] 命令來選取基本磁碟，並將焦點轉移到其上。

## <a name="BKMK_examples"></a>典型

若要將基本光碟從 MBR 磁碟分區樣式轉換為 GPT 磁碟分割樣式，請輸入：
```
convert gpt
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


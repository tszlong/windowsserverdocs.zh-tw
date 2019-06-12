---
title: convert basic
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: df1f999499154366304d59e0573ba921ab1af83d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434247"
---
# <a name="convert-basic"></a>convert basic



將空的動態磁碟轉換成基本磁碟。

如需如何使用此命令的相關資訊的指示，請參閱[變更為基本磁碟的動態磁碟上一步](https://go.microsoft.com/fwlink/?LinkId=207048)(https://go.microsoft.com/fwlink/?LinkId=207048)。

## <a name="syntax"></a>語法

```
convert basic [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|

## <a name="remarks"></a>備註

> [!IMPORTANT]
> 磁碟必須是空的才能將它轉換成基本磁碟。 備份您的資料，然後再刪除轉換磁碟之前的 所有資料分割或磁碟區。
> -   這項作業成功時，必須選取動態磁碟。 使用**選取磁碟**命令來選取動態磁碟，並將焦點移到它。

## <a name="BKMK_examples"></a>範例

若要將所選的動態磁碟轉換成 basic 中，輸入：
```
convert basic
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


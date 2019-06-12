---
title: convert gpt
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 433e30efeecec4e4ec51d67c40c14cacf986d12e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434221"
---
# <a name="convert-gpt"></a>convert gpt



將具有主開機記錄 (MBR) 磁碟分割樣式的空白基本磁碟轉換成具有 GUID 磁碟分割表格 (GPT) 磁碟分割樣式的基本磁碟。

如需如何使用此命令的相關資訊的指示，請參閱[GUID 磁碟分割表格磁碟變更為主要開機記錄磁碟](https://go.microsoft.com/fwlink/?LinkId=207049)(https://go.microsoft.com/fwlink/?LinkId=207049)。

## <a name="syntax"></a>語法

```
convert gpt [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|

## <a name="remarks"></a>備註

> [!IMPORTANT]
> 磁碟必須是空的才能將它轉換成 GPT 磁碟。 備份您的資料，然後再刪除轉換磁碟之前的 所有資料分割或磁碟區。
> -   轉換為 GPT 的所需的最小磁碟大小是 128 mb。
> -   這項作業成功時，必須選取一個基本 MBR 磁碟。 使用**選取磁碟**命令來選取基本磁碟，並將焦點移到它。

## <a name="BKMK_examples"></a>範例

若要從 MBR 磁碟分割樣式的基本光碟轉換成 GPT 磁碟分割樣式中，請輸入：
```
convert gpt
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


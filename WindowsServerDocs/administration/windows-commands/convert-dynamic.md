---
title: 轉換動態
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 353c1e4558ab2b0c948ec78c0cd87b579c738ec8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841609"
---
# <a name="convert-dynamic"></a>轉換動態



將基本磁碟轉換為動態磁碟。

如需如何使用此命令的相關資訊的指示，請參閱[將基本磁碟變更為動態磁碟](https://go.microsoft.com/fwlink/?LinkId=207047)(https://go.microsoft.com/fwlink/?LinkId=207047)。

## <a name="syntax"></a>語法

```
convert dynamic [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|

## <a name="remarks"></a>備註

-   基本磁碟上任何現有的資料分割成為簡單磁碟區。
-   這項作業成功時，必須選取基本磁碟。 使用**選取磁碟**命令來選取基本磁碟，並將焦點移到它。

## <a name="BKMK_examples"></a>範例

若要將基本磁碟轉換為動態磁碟，請輸入：
```
convert dynamic
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


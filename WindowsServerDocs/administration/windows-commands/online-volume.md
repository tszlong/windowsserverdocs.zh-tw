---
title: 線上磁碟區
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6bacba1e204f1eee2e3d4772ff9024aedbfc4fed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881999"
---
# <a name="online-volume"></a>線上磁碟區



將磁碟區目前離線為線上狀態

> [!IMPORTANT]
> 無法在任何版本的 Windows Vista 中使用此命令。

> [!IMPORTANT]
> 如果它使用唯讀磁碟區上，這個命令會失敗。

## <a name="syntax"></a>語法

```
online volume [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|

## <a name="remarks"></a>備註

-   此命令會失敗，失敗，或處於失敗的備援狀態的磁碟區上運作。
-   這個命令才會成功，就必須選取磁碟區。 使用**選取磁碟區**命令來選取磁碟區，並將焦點移到它。

## <a name="BKMK_examples"></a>範例

若要讓具有線上焦點的磁碟區，請輸入：
```
online volume
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


---
title: delete volume
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6d25ed68077f594c765cf5630648ad52528d8fe3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872509"
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
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|

## <a name="remarks"></a>備註

-   您無法刪除系統磁碟區、開機磁碟區或任何包含使用中分頁檔或損毀傾印 (記憶體傾印) 的磁碟區。
-   這項作業成功，就必須選取磁碟區。 使用**選取磁碟區**命令來選取磁碟區，並將焦點移到它。

## <a name="BKMK_examples"></a>範例

若要刪除具有焦點的磁碟區，請輸入：
```
delete volume
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


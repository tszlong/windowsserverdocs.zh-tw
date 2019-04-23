---
title: 離線的磁碟
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8fb9b3c3-0b2c-4192-a2e7-f706292653e3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 617371583a3f0cb3d0cb739845208e4216573d9c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834619"
---
# <a name="offline-disk"></a>離線的磁碟



需要具有焦點的線上磁碟為離線狀態。

> [!IMPORTANT]
> 無法在任何版本的 Windows Vista 中使用這個 DiskPart 命令。

## <a name="syntax"></a>語法

```
offline disk [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|

## <a name="remarks"></a>備註

-   此命令會在 SAN 線上模式中的磁碟上運作。 它會將其 SAN 模式變更為離線。
-   如果離線磁碟群組中的動態磁碟，磁碟的狀態會變更為**遺漏**和群組顯示為離線的磁碟。 遺失的磁碟移至無效的群組。 如果動態磁碟會在群組中，最後一個磁碟，則磁碟的狀態會變更為**離線**，將會移除空的群組。
-   必須選取一個磁碟**離線磁碟**命令才會成功。 使用**選取磁碟**命令來選取磁碟，並將焦點移到它。

## <a name="BKMK_examples"></a>範例

若要讓具有焦點的磁碟離線，請輸入：
```
offline disk
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


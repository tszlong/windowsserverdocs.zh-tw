---
title: 磁碟區離線
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b8f7192f-ea38-47d0-9d4e-58ef68336ae6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd14492a40046472f43f37d79c393c9467fe4a88
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873739"
---
# <a name="offline-volume"></a>磁碟區離線



需要具有焦點的線上磁碟區離線的狀態。

> [!IMPORTANT]
> 無法在任何版本的 Windows Vista 中使用這個 DiskPart 命令。

## <a name="syntax"></a>語法

```
offline volume [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|

## <a name="remarks"></a>備註

-   為了達成此目標必須選取一個磁碟區。 使用**選取磁碟區**命令來選取磁碟，並將焦點移到它。

## <a name="BKMK_examples"></a>範例

若要讓具有焦點的磁碟離線，請輸入：
```
offline volume
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


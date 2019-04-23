---
title: 刪除磁碟
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e401133854118e82a45fd5e01466288ae41f67da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863909"
---
# <a name="delete-disk"></a>刪除磁碟



從磁碟清單刪除遺失的動態磁碟。

如需如何使用此命令的相關資訊的指示，請參閱[移除遺失的動態磁碟](https://go.microsoft.com/fwlink/?LinkId=207055)(https://go.microsoft.com/fwlink/?LinkId=207055)。

## <a name="syntax"></a>語法

```
delete disk [noerr] [override]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|
|覆寫|啟用 DiskPart 來刪除磁碟上的全部簡單磁碟區。 如果磁碟包含鏡像磁碟區的下半部，則會刪除磁碟上的鏡像的部分。 如果磁碟屬於 raid-5 磁碟區，刪除磁碟覆寫命令將會失敗。|

## <a name="BKMK_examples"></a>範例

若要從磁碟清單刪除遺失的動態磁碟，請輸入：
```
delete disk
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


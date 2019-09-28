---
title: 刪除磁片
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 886e84edf80227d42ad77e36aed388b9e853ca62
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378674"
---
# <a name="delete-disk"></a>刪除磁片



從磁片清單刪除遺失的動態磁碟。

如需有關如何使用此命令的指示，請參閱[移除遺失的動態磁碟](https://go.microsoft.com/fwlink/?LinkId=207055)（ https://go.microsoft.com/fwlink/?LinkId=207055) 。

## <a name="syntax"></a>語法

```
delete disk [noerr] [override]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|
|覆寫|啟用 DiskPart 以刪除磁片上的所有簡單卷。 如果磁片包含一半的鏡像磁碟區，則會刪除磁片上的一半鏡像。 如果磁片是 RAID-5 磁片區的成員，則刪除磁片覆寫命令會失敗。|

## <a name="BKMK_examples"></a>典型

若要從磁片清單中刪除遺失的動態磁碟，請輸入：
```
delete disk
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


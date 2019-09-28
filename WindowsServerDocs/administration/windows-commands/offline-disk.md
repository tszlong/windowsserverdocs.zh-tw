---
title: 離線磁片
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f28d473cdb557d6adb3aaf235bebdfbc4e78b24a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372596"
---
# <a name="offline-disk"></a>離線磁片



將具有焦點的線上磁片帶到離線狀態。

> [!IMPORTANT]
> Windows Vista 的任何版本都無法使用此 DiskPart 命令。

## <a name="syntax"></a>語法

```
offline disk [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|

## <a name="remarks"></a>備註

-   此命令會在處於 SAN 線上模式的磁片上操作。 它會將其 SAN 模式變更為離線。
-   如果磁片群組中的動態磁碟已離線，則磁片的狀態會變更為 [**遺失**]，而群組會顯示離線的磁片。 遺失的磁片會移至不正確群組。 如果動態磁碟是群組中的最後一個磁片，則磁片的狀態會變更為 [**離線**]，而空的群組則會被移除。
-   必須選取磁片，[**離線磁片**] 命令才會成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。

## <a name="BKMK_examples"></a>典型

若要讓磁片具有離線焦點，請輸入：
```
offline disk
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)


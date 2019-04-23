---
title: 設定內容
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6f24e795f2d7c92d462cf822e70e4830b53827e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845849"
---
# <a name="set-contex"></a>設定內容



設定建立陰影複製的內容。 如果未指定參數，使用**設定內容**在命令提示字元中顯示說明。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|clientaccessible|指定陰影複製是供用戶端版本的 Windows。|
|持續性|指定陰影複製保存跨程式結束時、 重設或重新啟動。|
|Volatile|刪除陰影複製結束，或重設。|
|nowriters|指定會排除所有的寫入器。|

## <a name="remarks"></a>備註

-   *Clientaccessible*內容是依預設持續性。

## <a name="BKMK_examples"></a>範例

若要防止當您結束 DiskShadow 正在刪除陰影複製，請輸入：
```
set context persistent
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
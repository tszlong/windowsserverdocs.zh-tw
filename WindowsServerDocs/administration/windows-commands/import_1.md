---
title: 入口
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 569d986c57ae8b3d7253c050146ac0583c7c92df
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724839"
---
# <a name="import"></a>入口



將外部磁片群組匯入本機電腦的磁片群組。

## <a name="syntax"></a>語法

```
import [noerr]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|

## <a name="remarks"></a>備註

-   Import 命令會匯入與具有焦點的磁片位於相同群組中的每個磁片。
-   必須選取外部磁片群組中的動態磁碟，此操作才會成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。

## <a name="examples"></a>範例

若要匯入相同磁片群組中的每個磁片，並將焦點放入本機電腦的磁片群組，請輸入：
```
import
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)


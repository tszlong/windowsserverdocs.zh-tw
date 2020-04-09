---
title: 匯入
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7e098e7133bca18e1a6ba683e525783af17c3958
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842131"
---
# <a name="import"></a>匯入



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

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要匯入相同磁片群組中的每個磁片，並將焦點放入本機電腦的磁片群組，請輸入：
```
import
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)


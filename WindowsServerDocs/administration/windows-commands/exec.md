---
title: exec
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 514503e4920e16ba6778185af32f925541805223
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377429"
---
# <a name="exec"></a>exec



在本機電腦上執行檔案。 檔案可以是**cmd**腳本。

## <a name="syntax"></a>語法

```
exec <ScriptFile.cmd>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ScriptFile .cmd >|指定要執行的腳本檔案。|

## <a name="remarks"></a>備註

-   此命令是用來複製或還原資料，做為備份或還原順序的一部分。
-   如果腳本失敗，則會傳回錯誤並結束 DiskShadow。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
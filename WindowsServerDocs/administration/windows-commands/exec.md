---
title: exec
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ecdfd05b8abefb35946b783daaa3220a6713a38d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882919"
---
# <a name="exec"></a>exec



執行本機電腦上的檔案。 可以將檔案**cmd**指令碼。

## <a name="syntax"></a>語法

```
exec <ScriptFile.cmd>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ScriptFile.cmd>|指定要執行的指令碼檔案。|

## <a name="remarks"></a>備註

-   此命令用來複製或還原資料的備份或還原順序。
-   如果指令碼失敗，則會傳回錯誤，DiskShadow 會結束。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
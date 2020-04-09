---
title: exec
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d39fbf948050dd00f329e461c34c2365030cb05d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844991"
---
# <a name="exec"></a>exec



在本機電腦上執行檔案。 檔案可以是**cmd**腳本。

## <a name="syntax"></a>語法

```
exec <ScriptFile.cmd>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ScriptFile .cmd >|指定要執行的腳本檔案。|

## <a name="remarks"></a>備註

-   此命令是用來複製或還原資料，做為備份或還原順序的一部分。
-   如果腳本失敗，則會傳回錯誤並結束 DiskShadow。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
---
title: dispdiag
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b640883a207648d2ef6c9a7d6e5366cd0bb384c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377763"
---
# <a name="dispdiag"></a>dispdiag



記錄會將資訊顯示到檔案。

## <a name="syntax"></a>語法

```
dispdiag [-testacpi] [-d] [-delay <Seconds>] [-out <FilePath>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|- testacpi|執行熱鍵診斷測試。 顯示測試期間按下之任何金鑰的金鑰名稱、程式碼和掃描代碼。|
|-d.ddd...e|產生包含測試結果的傾印檔案。|
|-delay \<Seconds >|依指定的時間 *（以秒為單位）* 延遲資料收集。|
|-out \<FilePath >|指定路徑和檔案名，以儲存收集的資料。 這必須是最後一個參數。|
|-?|顯示可用的命令參數，並提供使用它們的協助。|
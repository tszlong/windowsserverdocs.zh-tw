---
title: dispdiag
description: Dispdiag 的參考主題，會將顯示資訊記錄到檔案中。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9f44e261b9c46157fb3e6bb7f9105af2480a60b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719402"
---
# <a name="dispdiag"></a>dispdiag

記錄會將資訊顯示到檔案。

## <a name="syntax"></a>語法

```
dispdiag [-testacpi] [-d] [-delay <Seconds>] [-out <FilePath>]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|- testacpi|執行熱鍵診斷測試。 顯示測試期間按下之任何金鑰的金鑰名稱、程式碼和掃描代碼。|
|-d|產生包含測試結果的傾印檔案。|
|-延遲\<秒數>|依指定的時間 *（以秒為單位）* 延遲資料收集。|
|-out \<FilePath>|指定路徑和檔案名，以儲存收集的資料。 這必須是最後一個參數。|
|-?|顯示可用的命令參數，並提供使用它們的協助。|
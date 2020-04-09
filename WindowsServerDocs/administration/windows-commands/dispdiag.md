---
title: dispdiag
description: 適用于 dispdiag 的 Windows 命令主題，會記錄將資訊顯示到檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 498294aa6678cc4904880128bca55d7900c91db5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845381"
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
|-延遲 \<秒數 >|依指定的時間 *（以秒為單位）* 延遲資料收集。|
|-out \<FilePath >|指定路徑和檔案名，以儲存收集的資料。 這必須是最後一個參數。|
|-?|顯示可用的命令參數，並提供使用它們的協助。|
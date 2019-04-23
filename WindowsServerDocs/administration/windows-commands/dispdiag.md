---
title: dispdiag
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9c96c70aac1b3329e050fa8b02743e61fed44d15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831459"
---
# <a name="dispdiag"></a>dispdiag



記錄檔會顯示資訊至檔案。

## <a name="syntax"></a>語法

```
dispdiag [-testacpi] [-d] [-delay <Seconds>] [-out <FilePath>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|- testacpi|會執行熱鍵診斷測試。 顯示索引鍵的名稱，在測試期間按下的任何索引鍵的程式碼，並掃描碼。|
|-d|產生測試結果的傾印檔案。|
|-delay\<秒 >|指定的時間，在所收集的資料會延遲*秒*。|
|-out \<FilePath>|指定路徑和檔案名稱來儲存收集的資料。 這必須是最後一個參數。|
|-?|顯示可用的命令參數，並提供說明使用它們。|
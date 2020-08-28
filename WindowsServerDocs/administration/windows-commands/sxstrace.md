---
title: sxstrace
description: 瞭解如何診斷並存問題。
ms.topic: reference
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a5398a9dcab96719de998a86bfa74df67a3f39b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024592"
---
# <a name="sxstrace"></a>sxstrace

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

診斷並存問題。

## <a name="syntax"></a>語法
```
sxstrace [{[trace -logfile:<FileName> [-nostop]|[parse -logfile:<FileName> -outfile:<ParsedFile>  [-filter:<AppName>]}]
```

#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|追蹤|啟用 (並存) 的 sxs 追蹤|
|-logfile|指定原始記錄檔。|
|\<FileName>|將追蹤記錄檔儲存至 *檔案名*。|
|-nostop|指定停止追蹤的提示。|
|parse|轉譯原始追蹤檔。|
|-outfile|指定輸出檔案名。|
|\<ParsedFile>|指定已剖析檔案的檔案名。|
|-filter|允許篩選輸出。|
|\<AppName>|指定應用程式的名稱。|
|stoptrace|如果之前未停止追蹤，請將它停止。|
|-?|在命令提示字元顯示說明。|

## <a name="examples"></a>範例
啟用追蹤，並將追蹤檔案儲存至 **sxstrace。 etl**：
```
sxstrace trace -logfile:sxstrace.etl
```
將原始追蹤檔轉譯成人類可讀取的格式，並將結果儲存至 **sxstrace.txt**：
```
sxstrace parse -logfile:sxstrace.etl -outfile:sxstrace.txt
```

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)


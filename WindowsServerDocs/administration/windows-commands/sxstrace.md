---
title: sxstrace
description: 瞭解如何診斷並存問題。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88ea6ba9d7a8f9744997eb78be2309693a6267b5
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821148"
---
# <a name="sxstrace"></a>sxstrace

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

診斷並存問題。

## <a name="syntax"></a>語法
```
sxstrace [{[trace -logfile:<FileName> [-nostop]|[parse -logfile:<FileName> -outfile:<ParsedFile>  [-filter:<AppName>]}]
```

#### <a name="parameters"></a>參數
|參數|說明|
|-------|--------|
|追蹤|啟用 sxs 的追蹤（並存）|
|-logfile|指定原始檔案的原始記錄檔。|
|\<檔案名>|將追蹤記錄儲存至*檔案名*。|
|-nostop|指定不提示停止追蹤。|
|parse|轉譯原始追蹤檔案。|
|-outfile|指定輸出檔案名。|
|\<ParsedFile>|指定已剖析檔案的檔案名。|
|-filter|允許篩選輸出。|
|\<AppName>|指定應用程式的名稱。|
|stoptrace|停止追蹤（如果尚未停止）。|
|-?|在命令提示字元顯示說明。|

## <a name="examples"></a>範例
啟用追蹤，並將追蹤檔案儲存至**sxstrace**：
```
sxstrace trace -logfile:sxstrace.etl
```
將原始追蹤檔案轉譯為人類看得懂的格式，並將結果儲存至**sxstrace**：
```
sxstrace parse -logfile:sxstrace.etl -outfile:sxstrace.txt
```

## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)


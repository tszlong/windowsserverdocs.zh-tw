---
title: sxstrace
description: 了解如何診斷並排顯示的問題。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 396d06bf079c0cfa8ba4864f71333eec39f7b255
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814099"
---
# <a name="sxstrace"></a>sxstrace

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

診斷並排顯示的問題。    

## <a name="syntax"></a>語法  
```  
sxstrace [{[trace /logfile:<FileName> [/nostop]|[parse /logfile:<FileName> /outfile:<ParsedFile>  [/filter:<AppName>]}]  
```  

### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|追蹤|啟用追蹤的 sxs （--並存）|  
|/logfile|指定的未經處理記錄檔。|  
|\<FileName>|儲存追蹤記錄*FileName*。|  
|/nostop|指定不提示，以停止追蹤。|  
|剖析|將轉譯的原始追蹤檔案。|  
|/outfile|指定輸出檔案名稱。|  
|\<ParsedFile>|指定的已剖析的檔案的檔名。|  
|/filter|可讓來篩選輸出。|  
|\<AppName>|指定應用程式的名稱。|  
|stoptrace|如果它不會停止之前，請停止追蹤。|  
|/?|在命令提示字元顯示說明。|  

## <a name="BKMK_Examples"></a>範例  
啟用追蹤，並儲存到追蹤檔案**sxstrace.etl**:  
```  
sxstrace trace /logfile:sxstrace.etl  
```  
將未經處理的追蹤檔案轉譯成一般人可讀取的格式，並儲存結果**sxstrace.txt**:  
```  
sxstrace parse /logfile:sxstrace.etl /outfile:sxstrace.txt  
```  

## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  

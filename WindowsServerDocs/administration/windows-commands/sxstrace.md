---
title: sxstrace
description: Systrace 命令的參考文章，可協助診斷並存問題。
ms.topic: reference
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 95f52993db56e61b3a1cd4d26a0ef36626421a15
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718205"
---
# <a name="sxstrace"></a>sxstrace

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

診斷並存問題。

## <a name="syntax"></a>語法

```
sxstrace [{[trace -logfile:<filename> [-nostop]|[parse -logfile:<filename> -outfile:<parsedfile>  [-filter:<appname>]}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| 追蹤 | 啟用並存追蹤。 |
| -logfile | 指定原始記錄檔。 |
| `<filename>` | 將追蹤記錄檔儲存至 `<filename` 。 |
| -nostop | 指定不應該收到停止追蹤的提示。 |
| parse | 轉譯原始追蹤檔。 |
| -outfile | 指定輸出檔案名。 |
| `<parsedfile>` | 指定已剖析檔案的檔案名。 |
| -filter | 允許篩選輸出。 |
| `<appname>` | 指定應用程式的名稱。 |
| stoptrace | 如果之前未停止追蹤，請將它停止。 |
| -? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要啟用追蹤，並將追蹤檔案儲存至 *sxstrace*，請輸入：

```
sxstrace trace -logfile:sxstrace.etl
```

若要將原始追蹤檔轉譯成人類可讀取的格式，並將結果儲存至 *sxstrace.txt*，請輸入：

```
sxstrace parse -logfile:sxstrace.etl -outfile:sxstrace.txt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

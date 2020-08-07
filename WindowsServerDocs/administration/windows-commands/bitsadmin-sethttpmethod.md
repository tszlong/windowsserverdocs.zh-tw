---
title: bitsadmin sethttpmethod
description: Bitsadmin setHTTPmethod 命令的參考文章，其會設定要使用的 HTTP 指令動詞。
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 9b782ea4f07113541ce62eaef63ac8047141e766
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881045"
---
# <a name="bitsadmin-sethttpmethod"></a>bitsadmin sethttpmethod

設定要使用的 HTTP 指令動詞。

## <a name="syntax"></a>語法

```
bitsadmin /sethttpmethod <job> <httpmethod>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| HTTPmethod | 要使用的 HTTP 指令動詞。 如需可用動詞的詳細資訊，請參閱[方法定義](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

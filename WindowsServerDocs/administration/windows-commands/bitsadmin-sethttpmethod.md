---
title: bitsadmin sethttpmethod
description: Bitsadmin setHTTPmethod 命令的參考文章，此命令會設定要使用的 HTTP 動詞命令。
ms.topic: reference
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 8374a8e306f88cbdbad079d99233171712cc00bc
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026266"
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
| HTTPmethod | 要使用的 HTTP 動詞命令。 如需可用動詞的詳細資訊，請參閱 [方法定義](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

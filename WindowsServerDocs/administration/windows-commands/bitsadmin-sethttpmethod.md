---
title: bitsadmin sethttpmethod
description: Bitsadmin setHTTPmethod 命令的參考文章，其會設定要使用的 HTTP 指令動詞。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 86d4749de294871a05176239cc1265974d8a7590
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927752"
---
# <a name="bitsadmin-sethttpmethod"></a>bitsadmin sethttpmethod

設定要使用的 HTTP 指令動詞。

## <a name="syntax"></a>語法

```
bitsadmin /sethttpmethod <job> <httpmethod>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| HTTPmethod | 要使用的 HTTP 指令動詞。 如需可用動詞的詳細資訊，請參閱[方法定義](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

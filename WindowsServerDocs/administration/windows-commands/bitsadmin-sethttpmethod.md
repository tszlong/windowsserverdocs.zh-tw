---
title: bitsadmin setHTTPmethod
description: 適用于**bitsadmin setHTTPmethod**的 Windows 命令主題，其會設定要使用的 HTTP 指令動詞。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: d349dcad7bdf6a6fc566ed961c3160836d7f49da
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122960"
---
# <a name="bitsadmin-sethttpmethod"></a>bitsadmin setHTTPmethod

設定要使用的 HTTP 指令動詞。

## <a name="syntax"></a>語法

```
bitsadmin /sethttpmethod <job> <httpmethod>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| HTTPmethod | 要使用的 HTTP 指令動詞。 如需可用動詞的詳細資訊，請參閱[方法定義](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
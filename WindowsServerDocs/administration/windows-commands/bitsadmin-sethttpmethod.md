---
title: bitsadmin sethttpmethod
description: Bitsadmin setHTTPmethod 命令的參考文章，此命令會設定要使用的 HTTP 動詞命令。
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 03/01/2019
ms.openlocfilehash: 0c8d2357e35831db89365eabbe0b97cdec88b6aa
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630878"
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

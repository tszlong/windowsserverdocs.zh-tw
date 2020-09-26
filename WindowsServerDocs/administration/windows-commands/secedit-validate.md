---
title: secedit validate
description: Secedit validate 命令的參考文章，可驗證儲存在安全性範本中的安全性設定。
ms.topic: reference
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 057573cf5bc28a47f5140fb58e23ee1ed4bbc4c1
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388097"
---
# <a name="secedit-validate"></a>secedit/validate

驗證儲存在安全性範本中的安全性設定 ( .inf 檔案) 。 驗證安全性範本可協助您判斷其中一個是否損毀或不當設定。 未套用已損毀或不當設定的安全性範本。

## <a name="syntax"></a>語法

```
secedit /validate <configuration file name>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<configuration file name>` | 必要。 指定要驗證之安全性範本的路徑和檔案名。 此命令不會更新記錄檔。 |

## <a name="examples"></a>範例

若要確認復原 .inf 檔案（ *secRBKcontoso*）在回復後仍然有效，請輸入：

```
secedit /validate secRBKcontoso.inf
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [secedit/analyze](secedit-analyze.md)

- [secedit/configure](secedit-configure.md)

- [secedit/export](secedit-export.md)

- [secedit/generaterollback](secedit-generaterollback.md)

- [secedit/import](secedit-import.md)
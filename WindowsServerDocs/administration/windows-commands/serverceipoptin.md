---
title: serverceipoptin
description: Serverceipoptin 的參考文章，可讓您參與客戶經驗改進計畫 (CEIP) 。
ms.topic: reference
ms.assetid: 3d7d7fa7-0689-4797-b802-36fe260d21a0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6df387158a9630ab767dda655346836355fae936
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389166"
---
# <a name="serverceipoptin"></a>serverceipoptin

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您參與客戶經驗改進計畫 (CEIP) 。

## <a name="syntax"></a>語法

```
serverceipoptin [/query] [/enable] [/disable]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /query | 驗證您目前的設定。 |
| /enable | 在 CEIP 中開啟您的參與。 |
| /disable | 在 CEIP 中關閉您的參與。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要驗證您目前的設定，請輸入：

```
serverceipoptin /query
```

若要開啟您的參與，請輸入：

```
serverceipoptin /enable
```

若要關閉您的參與，請輸入：

```
serverceipoptin /disable
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

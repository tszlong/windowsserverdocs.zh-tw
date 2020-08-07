---
title: secedit：驗證
description: '* * * * 的參考文章'
ms.topic: article
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30bb02f0d7947aa77f7ac41d5f7b179ca8fd236b
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882931"
---
# <a name="seceditvalidate"></a>secedit：驗證



驗證儲存在安全性範本中的安全性設定 ( .inf 檔案) 。

## <a name="syntax"></a>語法

```
Secedit /validate <configuration file name>

```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|組態檔名稱|必要。</br>指定要驗證之安全性範本的路徑和檔案名。|

## <a name="remarks"></a>備註

如果驗證安全性範本已損毀或設定不正確，則可協助您。

將不會套用不正確安全性範本。

記錄檔將不會更新。

在 Windows Server 2008 中，已 `Secedit /refreshpolicy` 取代為 `gpupdate` 。 如需有關如何重新整理安全性設定的詳細資訊，請參閱[Gpupdate](gpupdate.md)。

## <a name="examples"></a>範例

在安全性範本上執行復原之後，您想要確認復原 inf 檔案 secRBKcontoso 是有效的。
```
Secedit /validate secRBKcontoso.inf
```

## <a name="additional-references"></a>其他參考資料

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit](secedit.md)
- [命令列語法關鍵](command-line-syntax-key.md)
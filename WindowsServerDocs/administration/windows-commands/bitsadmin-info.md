---
title: bitsadmin info
description: Bitsadmin info 命令的參考文章，它會顯示指定作業的摘要資訊。
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6cd93716b818b3f1981ceb54c0c049933f25a77
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893724"
---
# <a name="bitsadmin-info"></a>bitsadmin info

顯示指定之作業的摘要資訊。

## <a name="syntax"></a>語法

```
bitsadmin /info <job> [/verbose]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| /verbose | 選擇性。 提供有關每項作業的詳細資訊。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的相關資訊：

```
bitsadmin /info myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin info](bitsadmin-info.md)

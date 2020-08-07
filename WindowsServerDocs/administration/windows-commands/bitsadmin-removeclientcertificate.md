---
title: bitsadmin removeclientcertificate
description: Bitsadmin removeclientcertificate 命令的參考文章，它會從作業中移除用戶端憑證。
ms.topic: article
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f4b2a17a08f3c2b224d90237975841644ea16ecd
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893380"
---
# <a name="bitsadmin-removeclientcertificate"></a>bitsadmin removeclientcertificate

從作業中移除用戶端憑證。

## <a name="syntax"></a>語法

```
bitsadmin /removeclientcertificate <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要從名為*myDownloadJob*的作業中移除用戶端憑證：

```
bitsadmin /removeclientcertificate myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

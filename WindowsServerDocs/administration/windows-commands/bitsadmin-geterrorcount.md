---
title: bitsadmin geterrorcount
description: Bitsadmin geterrorcount 命令的參考文章，它會抓取指定之作業產生暫時性錯誤的次數計數。
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8cfe0065ca9cec6d903466428b78cb8c103de5e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894345"
---
# <a name="bitsadmin-geterrorcount"></a>bitsadmin geterrorcount

抓取指定的作業產生暫時性錯誤的次數計數。

## <a name="syntax"></a>語法

```
bitsadmin /geterrorcount <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的錯誤計數資訊：

```
bitsadmin /geterrorcount myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

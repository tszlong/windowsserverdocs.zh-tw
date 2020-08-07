---
title: bitsadmin replaceremoteprefix
description: Bitsadmin replaceremoteprefix 命令的參考文章，它會視需要將作業中所有檔案的遠端 URL 從*oldprefix*變更為*newprefix*。
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 86149b09c65f430bf0a3bbe7a1595bb5385c6dcc
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893368"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

視需要將作業中所有檔案的遠端 URL 從*oldprefix*變更為*newprefix*。

## <a name="syntax"></a>語法

```
bitsadmin /replaceremoteprefix <job> <oldprefix> <newprefix>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| oldprefix | 現有的 URL 前置詞。 |
| newprefix | 新的 URL 前置詞。 |

## <a name="examples"></a>範例

若要變更名為*myDownloadJob*之作業中所有檔案的遠端 URL，請從 *http://stageserver* 到 *http://prodserver* 。

```
bitsadmin /replaceremoteprefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>其他資訊

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

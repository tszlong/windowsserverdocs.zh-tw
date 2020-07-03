---
title: bitsadmin getfilestotal
description: Bitsadmin getfilestotal 命令的參考文章，它會抓取指定之作業中的檔案數目。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7592f783d17e31fe8a1e7fbf82cb41e20171c9fd
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928280"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal

抓取指定之作業中的檔案數目。

## <a name="syntax"></a>語法

```
bitsadmin /getfilestotal <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取出包含在名為*myDownloadJob*之作業中的檔案數目：

```
bitsadmin /getfilestotal myDownloadJob
```

## <a name="see-also"></a>另請參閱

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

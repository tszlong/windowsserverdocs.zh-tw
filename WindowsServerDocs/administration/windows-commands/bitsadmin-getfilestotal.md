---
title: bitsadmin getfilestotal
description: Bitsadmin getfilestotal 命令的參考文章，可捕獲指定工作中的檔案數目。
ms.topic: reference
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 74e5dad863b12b7f90ed74bca0e6b0b352fb1360
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033606"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal

抓取指定作業中的檔案數目。

## <a name="syntax"></a>語法

```
bitsadmin /getfilestotal <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

取得名為 *myDownloadJob*的作業中包含的檔案數目：

```
bitsadmin /getfilestotal myDownloadJob
```

## <a name="see-also"></a>另請參閱

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

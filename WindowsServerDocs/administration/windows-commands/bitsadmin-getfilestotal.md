---
title: bitsadmin getfilestotal
description: Bitsadmin getfilestotal 命令的參考文章，可捕獲指定工作中的檔案數目。
ms.topic: reference
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 73a543914f31a7b9e5b028caf273d7945a954fdd
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632103"
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

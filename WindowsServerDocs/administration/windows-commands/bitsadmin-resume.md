---
title: bitsadmin resume
description: Bitsadmin resume 命令的參考文章，此命令會在傳送佇列中啟用新的或已暫停的工作。
ms.topic: reference
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2ccab242034af3e0b5e01f0efec60d5309879516
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631103"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume

在傳送佇列中啟用新的或已暫停的工作。 如果您不小心地繼續工作，或只需要暫停您的工作，您可以使用 [bitsadmin 暫](bitsadmin-suspend.md) 止參數來暫止作業。

## <a name="syntax"></a>語法

```
bitsadmin /resume <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要繼續名為 *myDownloadJob*的作業：

```
bitsadmin /resume myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 暫止命令](bitsadmin-suspend.md)

- [bitsadmin 命令](bitsadmin.md)

---
title: bitsadmin getdescription
description: Bitsadmin getdescription 命令的參考文章，此命令會抓取指定作業的描述。
ms.topic: reference
ms.assetid: f3974603-ebbe-4d31-8217-040fe2d90c85
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c8233ab420cadf3e7f578ce6f38d7631a8a1e820
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030416"
---
# <a name="bitsadmin-getdescription"></a>bitsadmin getdescription

抓取指定作業的描述。

## <a name="syntax"></a>語法

```
bitsadmin /getdescription <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的描述：

```
bitsadmin /getdescription myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

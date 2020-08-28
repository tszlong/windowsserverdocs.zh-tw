---
title: bitsadmin getproxylist-抓取指定作業的 proxy 清單。
description: Bitsadmin getproxylist 命令的參考文章，此命令會抓取指定作業的 proxy 清單。
ms.topic: reference
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9038f9faaedce30bfdf6025a40e3f6369805ac2a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024412"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

抓取要用於指定作業的 proxy 伺服器清單（以逗號分隔）。

## <a name="syntax"></a>語法

```
bitsadmin /getproxylist <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的 proxy 清單：

```
bitsadmin /getproxylist myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

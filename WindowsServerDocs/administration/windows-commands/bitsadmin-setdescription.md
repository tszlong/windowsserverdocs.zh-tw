---
title: bitsadmin setdescription
description: Bitsadmin setdescription 命令的參考文章，此命令會設定指定工作的描述。
ms.topic: reference
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 86f63a553b9d308ef3e8bfe5bfc2a2334b5d28e8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031256"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription

設定指定之作業的描述。

## <a name="syntax"></a>語法

```
bitsadmin /setdescription <job> <description>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| description | 用來描述作業的文字。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的描述：

```
bitsadmin /setdescription myDownloadJob music_downloads
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

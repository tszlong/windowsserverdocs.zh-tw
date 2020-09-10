---
title: bitsadmin getpeercachingflags
description: Bitsadmin getpeercachingflags 命令的參考文章，此命令會抓取旗標，以判斷是否可以快取和提供對等的作業檔案，以及 BITS 是否可以從對等下載作業的內容。
ms.topic: reference
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6ec765183543bf42152d198c10ebc5debfa81edb
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631844"
---
# <a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取旗標，這些旗標會決定是否可以快取作業的檔案，並將其提供給對等，而且 BITS 是否可以從對等下載作業的內容。

## <a name="syntax"></a>語法

```
bitsadmin /getpeercachingflags <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的旗標：

```
bitsadmin /getpeercachingflags myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

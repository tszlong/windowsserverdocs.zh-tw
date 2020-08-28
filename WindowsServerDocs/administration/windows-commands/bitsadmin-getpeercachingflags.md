---
title: bitsadmin getpeercachingflags
description: Bitsadmin getpeercachingflags 命令的參考文章，此命令會抓取旗標，以判斷是否可以快取和提供對等的作業檔案，以及 BITS 是否可以從對等下載作業的內容。
ms.topic: reference
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c6d7b53dc9c9ff99188b98d19e1418cbf9f1656
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028716"
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

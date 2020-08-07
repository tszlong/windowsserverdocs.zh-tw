---
title: bitsadmin getpeercachingflags
description: Bitsadmin getpeercachingflags 命令的參考文章，它會抓取旗標來判斷作業的檔案是否可以快取並提供給對等，以及 BITS 是否可以從對等電腦下載作業的內容。
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ae22862c91fc9911bbf202dd51e52578b410995b
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894025"
---
# <a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取旗標，以判斷作業的檔案是否可以快取並提供給對等，以及 BITS 是否可以從對等電腦下載作業的內容。

## <a name="syntax"></a>語法

```
bitsadmin /getpeercachingflags <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的旗標：

```
bitsadmin /getpeercachingflags myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

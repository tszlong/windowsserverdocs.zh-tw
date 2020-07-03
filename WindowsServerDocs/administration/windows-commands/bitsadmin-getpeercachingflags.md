---
title: bitsadmin getpeercachingflags
description: Bitsadmin getpeercachingflags 命令的參考文章，它會抓取旗標來判斷作業的檔案是否可以快取並提供給對等，以及 BITS 是否可以從對等電腦下載作業的內容。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad270fb8003c518c43bae86b066fea5ebc31d008
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926869"
---
# <a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取旗標，以判斷作業的檔案是否可以快取並提供給對等，以及 BITS 是否可以從對等電腦下載作業的內容。

## <a name="syntax"></a>語法

```
bitsadmin /getpeercachingflags <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
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

---
title: bitsadmin getpeercachingflags
description: 適用于**bitsadmin getpeercachingflags**的 Windows 命令主題，它會抓取旗標來判斷作業的檔案是否可以快取並提供給對等，以及 BITS 是否可以從對等電腦下載作業的內容。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 59ff3e3a5802c6023d85129c82f19cd7ee625d2f
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123126"
---
# <a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取旗標，以判斷作業的檔案是否可以快取並提供給對等，以及 BITS 是否可以從對等電腦下載作業的內容。

## <a name="syntax"></a>語法

```
bitsadmin /getpeercachingflags <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會抓取名為*myDownloadJob*之作業的旗標。

```
C:\>bitsadmin /getpeercachingflags myJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
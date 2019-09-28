---
title: bitsadmin cache 和 deleteurl
description: Bitsadmin 快取**和 deleteurl**的 Windows 命令主題-刪除指定 URL 的所有快取專案。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 869d3bc0f011cc82aaea9b7468667964051e1c00
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382064"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>bitsadmin cache 和 deleteurl



刪除指定 URL 的所有快取專案。

## <a name="syntax"></a>語法

```
bitsadmin /DeleteURL url
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|url|識別遠端檔案的統一資源定位器。|

## <a name="BKMK_examples"></a>典型

下列範例會刪除 https://www.microsoft.com/en/us/default.aspx 的所有快取專案
```
C:\>bitsadmin /DeleteURL https://www.microsoft.com/en/us/default.aspx 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
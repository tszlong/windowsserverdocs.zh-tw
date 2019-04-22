---
title: bitsadmin 快取和 deleteurl
description: 適用於 Windows 命令主題**bitsadmin 快取和 deleteurl** -刪除所有的快取項目，為指定的 URL。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a831c49e1461761cb7466b46e7a5ad8e037f4ec9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816649"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>bitsadmin 快取和 deleteurl



刪除所有的快取項目，為指定的 URL。

## <a name="syntax"></a>語法

```
bitsadmin /DeleteURL url
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|url|統一資源定位器，識別遠端檔案。|

## <a name="BKMK_examples"></a>範例

下列範例會刪除所有快取項目 https://www.microsoft.com/en/us/default.aspx
```
C:\>bitsadmin /DeleteURL https://www.microsoft.com/en/us/default.aspx 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
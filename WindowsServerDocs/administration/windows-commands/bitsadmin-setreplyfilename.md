---
title: bitsadmin setreplyfilename
description: '**Bitsadmin setreplyfilename**的 Windows 命令主題-指定包含伺服器回復之檔案的路徑。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a490b5bc565549d096b6f43f42758f77570fcb26
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380419"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

指定包含伺服器回復之檔案的路徑。

**BITS 1.2 和更早版本**： 未支援。

## <a name="syntax"></a>語法

```
bitsadmin /SetReplyFileName <Job> <Path>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|`Path`|放置伺服器回復的位置|

## <a name="remarks"></a>備註

僅適用于上傳-回復作業。

## <a name="BKMK_examples"></a>典型

下列範例會針對名為*myDownloadJob*的作業設定回復檔案名 pathfor。
```
C:\>bitsadmin /SetReplyFileName myDownloadJob c:\reply
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
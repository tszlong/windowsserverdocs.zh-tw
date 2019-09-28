---
title: bitsadmin setcustomheaders
description: 適用于**bitsadmin setcustomheaders**的 Windows 命令主題-將自訂 HTTP 標頭新增至 GET 要求。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45e3a5178df69b84618966ca0fcd9cc1e6d0e449
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380636"
---
# <a name="bitsadmin-setcustomheaders"></a>bitsadmin setcustomheaders



將自訂 HTTP 標頭新增至 GET 要求。

## <a name="syntax"></a>語法

```
bitsadmin /SetCustomHeaders <Job> <Header1> <Header2> <. . .>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|Header1.xml H 2。 . .|作業的自訂標頭|

## <a name="remarks"></a>備註

-   這個參數是用來將自訂 HTTP 標頭新增至傳送至 HTTP 伺服器的 GET 要求。

## <a name="BKMK_examples"></a>典型

下列範例會針對名為*myDownloadJob*的作業新增自訂 HTTP 標頭。
```
C:\>bitsadmin / SetCustomHeaders myDownloadJob "Accept-encoding:deflate/gzip"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
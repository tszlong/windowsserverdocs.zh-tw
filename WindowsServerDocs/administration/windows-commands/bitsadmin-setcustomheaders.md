---
title: bitsadmin setcustomheaders
description: 適用於 Windows 命令主題**bitsadmin setcustomheaders** -將自訂的 HTTP 標頭新增至 GET 要求。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6d90ac2d23b852ae0c2114e7cd5a9c9e6382ce8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853849"
---
# <a name="bitsadmin-setcustomheaders"></a>bitsadmin setcustomheaders



將自訂的 HTTP 標頭新增至 GET 要求。

## <a name="syntax"></a>語法

```
bitsadmin /SetCustomHeaders <Job> <Header1> <Header2> <. . .>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|Header1 Header2。 . .|自訂標頭的作業|

## <a name="remarks"></a>備註

-   這個參數用來將自訂的 HTTP 標頭新增至 GET 要求傳送至 HTTP 伺服器。

## <a name="BKMK_examples"></a>範例

下列範例會將名為工作的自訂 HTTP 標頭*myDownloadJob*。
```
C:\>bitsadmin / SetCustomHeaders myDownloadJob "Accept-encoding:deflate/gzip"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
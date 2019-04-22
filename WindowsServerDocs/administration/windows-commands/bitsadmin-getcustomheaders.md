---
title: bitsadmin getcustomheaders
description: 適用於 Windows 命令主題**bitsadmin getcustomheaders** -擷取來自工作的自訂 HTTP 標頭。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f5959541f0e3190e26bbb298a9cd7c63ab32cae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812089"
---
# <a name="bitsadmin-getcustomheaders"></a>bitsadmin getcustomheaders



從作業中擷取自訂的 HTTP 標頭。

## <a name="syntax"></a>語法

```
bitsadmin /GetCustomHeaders <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例

下列範例會取得名為工作的自訂標頭*myDownloadJob*。
```
C:\>bitsadmin /GetCustomHeaders myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
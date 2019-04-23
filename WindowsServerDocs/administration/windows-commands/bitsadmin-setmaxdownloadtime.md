---
title: bitsadmin setmaxdownloadtime
description: 適用於 Windows 命令主題**bitsadmin setmaxdownloadtime** -設定下載逾時 （秒）。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f13b44429bec2718af1a648f273fead18d4e9e08
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830989"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>bitsadmin setmaxdownloadtime



設定下載逾時 （秒）。

## <a name="syntax"></a>語法

```
bitsadmin /SetMaxDownloadTime <Job> <Timeout>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|逾時|以秒為單位的逾時|

## <a name="remarks"></a>備註

-   N/A

## <a name="BKMK_examples"></a>範例

下列範例會設定名為作業的逾時*myDownloadJob*為 10 秒。
```
C:\>bitsadmin /SetMaxDownloadTime myDownloadJob 10
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
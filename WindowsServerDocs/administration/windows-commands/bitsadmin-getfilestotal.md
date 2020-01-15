---
title: bitsadmin getfilestotal
description: '**Bitsadmin getfilestotal**的 Windows 命令主題-抓取指定之作業中的檔案數目。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5c28d8e970bd7db896073bf8cddb168ffe9deff
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75946839"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal



抓取指定之作業中的檔案數目。

## <a name="syntax"></a>語法

```
bitsadmin /GetFilesTotal <Job>
```

## <a name="parameters"></a>Parameters

|參數|說明|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例

下列範例會抓取名為*myDownloadJob*之作業中包含的檔案數目。
```
C:\>bitsadmin /GetFilesTotal myDownloadJob
```

## <a name="see-also"></a>請參閱

[命令列語法關鍵](command-line-syntax-key.md)

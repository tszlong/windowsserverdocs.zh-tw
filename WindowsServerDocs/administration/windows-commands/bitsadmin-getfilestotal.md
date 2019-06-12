---
title: bitsadmin getfilestotal
description: 適用於 Windows 命令主題**bitsadmin getfilestotal** -擷取的檔案中指定的作業數目。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b5f6d32b3410b182c510cf40b9def5370efafdc4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435126"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal



擷取指定的作業中的檔案數目。

## <a name="syntax"></a>語法

```
bitsadmin /GetFilesTotal <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例

下列範例會擷取包含在名為作業的檔案數目*myDownloadJob*。
```
C:\>bitsadmin /GetFilesTotal myDownloadJob
```

# #

[命令列語法重點](command-line-syntax-key.md)另請參閱
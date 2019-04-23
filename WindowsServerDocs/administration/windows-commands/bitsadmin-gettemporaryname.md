---
title: bitsadmin gettemporaryname
description: 適用於 Windows 命令主題**bitsadmin gettemporaryname** -報告作業內的指定檔案的暫存檔案名稱。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 762a2a5943202b38e94a245b74745e6631e0792d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876709"
---
# <a name="bitsadmin-gettemporaryname"></a>bitsadmin gettemporaryname



報告指定的檔案，作業內的暫存檔案名稱。

## <a name="syntax"></a>語法

```
bitsadmin /GetTemporaryName <Job> <file index> 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|檔案索引|會從 0 開始|

## <a name="BKMK_examples"></a>範例

下列範例會報告名為工作的檔案 2 暫存檔名*myJob*。
```
C:\>bitsadmin /GetTemporaryName myJob 1 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
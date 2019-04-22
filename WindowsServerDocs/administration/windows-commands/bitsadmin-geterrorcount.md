---
title: bitsadmin geterrorcount
description: 適用於 Windows 命令主題**bitsadmin geterrorcount** -擷取指定的作業產生的暫時性錯誤的次數的計數。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91045372931efec0e3189132a275eeacab584de4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818369"
---
# <a name="bitsadmin-geterrorcount"></a>bitsadmin geterrorcount



擷取指定的作業產生的暫時性錯誤的次數的計數。

## <a name="syntax"></a>語法

```
bitsadmin /GetErrorCount <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業計數錯誤資訊*myDownloadJob*。
```
C:\>bitsadmin /GetErrorCount myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
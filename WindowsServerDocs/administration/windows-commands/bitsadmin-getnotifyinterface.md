---
title: bitsadmin getnotifyinterface
description: 適用于**bitsadmin getnotifyinterface**的 Windows 命令主題-判斷另一個程式是否已為指定的作業註冊 COM 回呼介面。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 826e13cf8a3e54935ceb5a72ff82647cacfc3be5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381472"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

判斷另一個程式是否已為指定的作業註冊 COM 回呼介面（通知介面）。

## <a name="syntax"></a>語法

```
bitsadmin /GetNotifyInterface <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

顯示已註冊或已取消註冊。

> [!NOTE]
> 您無法判斷註冊回呼介面的程式。

## <a name="BKMK_examples"></a>典型

下列範例會針對名為*myDownloadJob*的作業，抓取通知介面。
```
C:\>bitsadmin /GetNotifyInterface myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
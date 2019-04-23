---
title: bitsadmin takeownership
description: 適用於 Windows 命令主題**bitsadmin takeownership** -可讓使用者以系統管理權限取得指定工作的擁有權。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aedca49e43588ab51f84477cf8690cf58486c3cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827839"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership



可讓使用者以系統管理權限取得指定工作的擁有權。

## <a name="syntax"></a>語法

```
bitsadmin /TakeOwnership <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例

下列範例會取得名為之作業的擁有權*myDownloadJob*。
```
C:\>bitsadmin /TakeOwnership myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
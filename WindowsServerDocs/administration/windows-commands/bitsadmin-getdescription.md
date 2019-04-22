---
title: bitsadmin getdescription
description: 適用於 Windows 命令主題**bitsadmin getdescription** -擷取指定工作的描述。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3974603-ebbe-4d31-8217-040fe2d90c85
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ee20dd808cdbc8b76f44b7b14c9fd65b313a74e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813129"
---
# <a name="bitsadmin-getdescription"></a>bitsadmin getdescription



擷取指定工作的描述。

## <a name="syntax"></a>語法

```
bitsadmin /GetDescription <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業的描述*myDownloadJob*。
```
C:\>bitsadmin /GetDescription myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
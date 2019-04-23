---
title: bitsadmin info
description: 適用於 Windows 命令主題**顯示指定工作的摘要資訊。** -bitsadmin 資訊
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ee96c69e311600a53f04b1b883983718adf0f69
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851519"
---
# <a name="bitsadmin-info"></a>bitsadmin info



顯示指定工作的摘要資訊。

## <a name="syntax"></a>語法

```
bitsadmin /Info <Job> [/verbose]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

使用 / verbose 參數，以提供有關工作的詳細的資訊。

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為工作的相關資訊*myDownloadJob*。
```
C:\>bitsadmin /Info myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
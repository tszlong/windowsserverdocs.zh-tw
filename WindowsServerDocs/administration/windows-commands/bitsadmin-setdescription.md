---
title: bitsadmin setdescription
description: '**Bitsadmin setdescription**的 Windows 命令主題-設定指定工作的描述。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d140ee9d575828a1a4d536073e468c9b4e56799f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380935"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription



設定指定之作業的描述。

## <a name="syntax"></a>語法

```
bitsadmin /SetDescription <Job> <Description>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|描述|用來描述作業的文字。|

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*之作業的描述。
```
C:\>bitsadmin /SetDescription myDownloadJob "Music Downloads"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
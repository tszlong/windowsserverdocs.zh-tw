---
title: bitsadmin getowner
description: '**Bitsadmin getowner**的 Windows 命令主題-抓取指定之作業的擁有者。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab8151ba8b1379c101aa037504ae2021ff0df62f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381454"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

顯示指定作業之擁有者的顯示名稱或 GUID。

## <a name="syntax"></a>語法

```
bitsadmin /GetOwner <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型

下列範例會顯示名為*myDownloadJob*之作業的擁有者。
```
C:\>bitsadmin /GetOwner myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
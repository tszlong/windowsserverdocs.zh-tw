---
title: bitsadmin getreplydata
description: '**Bitsadmin getreplydata**的 Windows 命令主題-以十六進位格式抓取伺服器的回復資料。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 819f97d5-b255-4b2d-9f63-0daa73915434
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7ebd3ee77e5d442467f49bb209c560f089f2271b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381285"
---
# <a name="bitsadmin-getreplydata"></a>bitsadmin getreplydata

以十六進位格式抓取伺服器的回復資料。

**BITS 1.2 和更早版本**： 未支援。

## <a name="syntax"></a>語法

```
bitsadmin /GetReplyData <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

僅適用于上傳-回復作業。

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*之作業的回復資料。
```
C:\>bitsadmin /GetReplyData myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
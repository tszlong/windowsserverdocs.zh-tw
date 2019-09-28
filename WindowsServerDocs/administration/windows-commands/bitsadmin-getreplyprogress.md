---
title: bitsadmin getreplyprogress
description: '**Bitsadmin getreplyprogress**的 Windows 命令主題-抓取伺服器回復的大小和進度。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f7cb0b4-ad95-44fd-a35d-0ddf5fc0b0d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c791fe98271b497e5ecf48338ab3bbb0cc50de98
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381236"
---
# <a name="bitsadmin-getreplyprogress"></a>bitsadmin getreplyprogress

抓取伺服器回復的大小和進度。

**BITS 1.2 和更早版本**： 未支援。

## <a name="syntax"></a>語法

```
bitsadmin /GetReplyProgress <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

僅適用于上傳-回復作業。

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*之作業的回復進度。
```
C:\>bitsadmin /GetReplyProgress myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
---
title: bitsadmin rawreturn
description: '**Bitsadmin rawreturn**的 Windows 命令主題-傳回適用于剖析的資料。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 86d769de460538acda696194348980de5752d6d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380883"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

傳回適用于剖析的資料。

## <a name="syntax"></a>語法

```
bitsadmin /RawReturn
```

## <a name="remarks"></a>備註

從輸出中去除分行符號和格式。

一般來說，您會將此命令與**Create**和**Get\\** * 參數搭配使用，以只接收值。 您必須在其他參數之前指定此參數。

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*之作業狀態的原始資料。
```
C:\>bitsadmin /RawReturn /GetState myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
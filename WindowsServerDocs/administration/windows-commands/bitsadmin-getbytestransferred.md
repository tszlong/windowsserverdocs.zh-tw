---
title: bitsadmin getbytestransferred
description: 適用于**bitsadmin getbytestransferred**的 Windows 命令主題-抓取針對指定之作業所傳輸的位元組數目。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47bbf184-e06f-4be0-b2ba-d32b10d82002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f690fa55a4ac5ae31223794c5e7eabc0c982c2ce
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381729"
---
# <a name="bitsadmin-getbytestransferred"></a>bitsadmin getbytestransferred



抓取指定作業所傳輸的位元組數目。

## <a name="syntax"></a>語法

```
bitsadmin /GetBytesTransferred <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型

下列範例會針對名為*myDownloadJob*的作業，抓取所傳輸的位元組數目。
```
C:\>bitsadmin /GetBytesTransferred myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
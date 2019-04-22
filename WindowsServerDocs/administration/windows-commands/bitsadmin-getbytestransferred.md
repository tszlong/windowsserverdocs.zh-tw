---
title: bitsadmin getbytestransferred
description: 適用於 Windows 命令主題**bitsadmin getbytestransferred** -擷取指定作業所傳輸的位元組數目。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cce2c051af169385c43fdff4efdeff46d8422926
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814609"
---
# <a name="bitsadmin-getbytestransferred"></a>bitsadmin getbytestransferred



擷取指定作業所傳送的位元組數目。

## <a name="syntax"></a>語法

```
bitsadmin /GetBytesTransferred <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業所傳送的位元組數目*myDownloadJob*。
```
C:\>bitsadmin /GetBytesTransferred myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
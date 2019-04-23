---
title: bitsadmin gettype
description: 適用於 Windows 命令主題**bitsadmin gettype** -擷取指定作業的作業類型。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff0118f14acbf4e9f37c02e660bd9c7f6e8d0f70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879429"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype



擷取指定工作的作業類型。

## <a name="syntax"></a>語法

```
bitsadmin /GetType <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

型別可以下載上, 傳，請上傳-回覆或未知。

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業的作業類型*myDownloadJob*。
```
C:\>bitsadmin /GetType myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
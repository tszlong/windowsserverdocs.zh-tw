---
title: bitsadmin suspend
description: 適用於 Windows 命令主題**bitsadmin 暫停**-暫停指定的工作。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87e1bbd1b068d68fb60655043735c6c1aeb07707
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825919"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> 適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

暫停指定的工作。

## <a name="syntax"></a>語法

```
bitsadmin /Suspend <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

若要重新啟動作業，請使用[bitsadmin 繼續](bitsadmin-resume.md)切換。

## <a name="BKMK_examples"></a>範例

下列範例會暫停工作名為*myDownloadJob*。

```
C:\>bitsadmin /Suspend myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

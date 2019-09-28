---
title: bdehdcfg newdriveletter
description: 適用于 bdehdcfg newdriveletter 的 Windows 命令主題-將新的磁碟機號指派給用來作為系統磁片磁碟機的磁片磁碟機部分。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2abd4a686f358b5dd844514735edb3ffaa13845
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382226"
---
# <a name="bdehdcfg-newdriveletter"></a>bdehdcfg： newdriveletter



將新的磁碟機號指派給用來做為系統磁片磁碟機的磁片磁碟機部分。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DriveLetter >|定義將指派給指定目標磁片磁碟機的磁碟機號。|

## <a name="remarks"></a>備註

最佳做法是，建議您不要將磁碟機號指派給您的系統磁片磁碟機。

## <a name="BKMK_Examples"></a>典型

下列範例顯示指派磁碟機號 P 的預設磁片磁碟機。
```
bdehdcfg -target default -newdriveletter P:
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)
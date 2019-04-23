---
title: bdehdcfg newdriveletter
description: Bdehdcfg newdriveletter-Windows 命令主題會將新的磁碟機代號指派給做為系統磁碟機的磁碟機的部分。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cd40942dfb724d46c0fa9a43c4646e1db09d2a76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887139"
---
# <a name="bdehdcfg-newdriveletter"></a>Bdehdcfg: newdriveletter



將新的磁碟機代號指派給 做為系統磁碟機的磁碟機的部分。 如需如何使用此命令的範例，請參閱 <<c0> [ 範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DriveLetter>|定義要指派給指定的目標磁碟機的磁碟機代號。|

## <a name="remarks"></a>備註

最佳做法，建議您不要指派磁碟機代號為您的系統磁碟機。

## <a name="BKMK_Examples"></a>範例

下列範例顯示的磁碟機代號 P.指派預設磁碟機
```
bdehdcfg -target default -newdriveletter P:
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)
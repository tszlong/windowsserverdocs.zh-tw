---
title: bdehdcfg 大小
description: 當正在建立新的系統磁碟機，Windows 命令主題-指定系統磁碟分割的大小。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 80f55b1d-a28d-4edf-9997-1fb918b7b5a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d024bb4092f93782300d6afb9053cee1da32629a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817519"
---
# <a name="bdehdcfg-size"></a>bdehdcfg： 大小



當正在建立新的系統磁碟機，請指定系統磁碟分割的大小。 如需如何使用此命令的範例，請參閱 <<c0> [ 範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink} -size <SizeinMB>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<SizeinMB>|指出百萬位元組 (MB)，要用於新的資料分割數目。|

## <a name="remarks"></a>備註

如果您未指定大小，此工具會使用 300 MB 的預設值。 系統磁碟機的最小大小為 100 MB。 如果您將會在系統磁碟分割上儲存系統復原或其他系統工具，您應該據以增加的大小。

> [!NOTE]
> **大小**不得搭配命令**目標**\<磁碟機代號 >**合併**命令。

## <a name="BKMK_Examples"></a>範例

下列範例說明如何利用**大小**配置 500 MB 的預設系統磁碟機的命令。
```
bdehdcfg -target default -size 500
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)
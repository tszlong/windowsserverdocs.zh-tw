---
title: bdehdcfg 目標
description: Bdehdcfg 目標-Windows 命令主題用於系統磁碟機的 BitLocker 和 Windows 復原準備的磁碟分割。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f761d25d-8349-4ac7-ac46-6bb340a4348f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8d180974f480b4c40532dab529ad49dcc33540d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881529"
---
# <a name="bdehdcfg-target"></a>bdehdcfg: target



準備用於系統磁碟機的 BitLocker 和 Windows 復原的資料分割。 根據預設，此資料分割會建立不含磁碟機代號。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|預設值|表示命令列工具將遵循相同的程序，BitLocker 安裝精靈。|
|未配置|在磁碟上建立系統磁碟分割未配置的可用空間不足。|
|\<磁碟機代號 > 壓縮|減少指定所建立的作用中的系統磁碟分割所需數量的磁碟機。 若要使用此命令，指定的磁碟機必須至少 5%的可用空間。|
|\<磁碟機代號 > 合併|使用指定為作用中的系統磁碟分割的磁碟機。 作業系統磁碟機不能合併的目標。|

## <a name="BKMK_Examples"></a>範例

下列範例會說明使用**目標**命令，以將現有的磁碟機 (P) 指定為系統磁碟機。
```
bdehdcfg -target P: merge
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)
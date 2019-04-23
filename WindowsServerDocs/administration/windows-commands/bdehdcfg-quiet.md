---
title: bdehdcfg quiet
description: Windows 命令主題為 bdehdcfg 無訊息模式-告訴 bdehdcfg 表示不顯示所有動作和錯誤。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f0d98f6ae76e9bf6357689c97e091766b9645c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865749"
---
# <a name="bdehdcfg-quiet"></a>bdehdcfg: quiet



會通知 Bdehdcfg 命令列工具的所有動作和錯誤不會顯示在命令列介面。 如需如何使用此命令的範例，請參閱 <<c0> [ 範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -quiet
```

### <a name="parameters"></a>參數

此命令會接受任何其他參數。

## <a name="remarks"></a>備註

如果任何 Yes/任何 （是/否） 提示會顯示在磁碟機準備期間，會假設 「 是 」 回應。 若要檢視磁碟機準備期間發生任何錯誤，請檢閱系統事件記錄檔底下**Microsoft Windows-BitLocker DrivePreparationTool**事件提供者。

## <a name="BKMK_Examples"></a>範例

下列範例說明如何利用**安靜**命令。
```
bdehdcfg -target default -quiet
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)
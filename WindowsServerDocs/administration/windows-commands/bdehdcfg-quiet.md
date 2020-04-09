---
title: bdehdcfg quiet
description: '**Bdehdcfg quiet**的 Windows 命令主題，告訴 bdehdcfg 不顯示所有動作和錯誤。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e9c24d8861476e6c1578af8245236d699b6ef6db
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851041"
---
# <a name="bdehdcfg-quiet"></a>bdehdcfg： quiet

通知 Bdehdcfg 命令列工具，所有動作和錯誤都不會顯示在命令列介面中。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -quiet
```

#### <a name="parameters"></a>參數

此命令不接受任何其他參數。

## <a name="remarks"></a>備註

如果在磁片磁碟機準備期間顯示任何 [是/否] （Y/N）提示，則會假設為「是」答案。 若要查看磁片磁碟機準備期間發生的任何錯誤，請參閱**DrivePreparationTool**事件提供者底下的系統事件記錄檔。

## <a name="examples"></a><a name="BKMK_Examples"></a>典型

下列範例說明如何使用**quiet**命令。

```
bdehdcfg -target default -quiet
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Bdehdcfg](bdehdcfg.md)
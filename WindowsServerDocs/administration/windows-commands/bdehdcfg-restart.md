---
title: bdehdcfg 重新開機
description: '**Bdehdcfg restart**的 Windows 命令主題，它會告訴 bdehdcfg，電腦應該在磁片磁碟機準備結束後重新開機。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ae6f8d31c09feddf8f994c28d34e4e1b08cc322
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851031"
---
# <a name="bdehdcfg-restart"></a>bdehdcfg：重新開機

通知 Bdehdcfg 命令列工具，電腦應該在磁片磁碟機準備結束後重新開機。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -restart
```

#### <a name="parameters"></a>參數

此命令不接受任何其他參數。

## <a name="remarks"></a>備註

如果有其他使用者登入電腦，但未指定 [ **quiet** ] 命令，則會顯示提示，確認電腦是否應重新開機。

## <a name="examples"></a><a name="BKMK_Examples"></a>典型

下列範例說明如何使用**重新開機**命令。

```
bdehdcfg -target default -restart
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Bdehdcfg](bdehdcfg.md)
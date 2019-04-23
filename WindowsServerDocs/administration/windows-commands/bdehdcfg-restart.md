---
title: bdehdcfg 重新啟動
description: Windows 命令主題 bdehdcfg restart-會告訴 bdehdcfg 的磁碟機準備已完成之後，應該重新啟動電腦。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f361db8fdf33bd414556575de75241f7dbd9327
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879459"
---
# <a name="bdehdcfg-restart"></a>bdehdcfg: restart



通知 Bdehdcfg 命令列工具的磁碟機準備已完成之後，應該重新啟動電腦。 如需如何使用此命令的範例，請參閱 <<c0> [ 範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -restart
```

### <a name="parameters"></a>參數

此命令會接受任何其他參數。

## <a name="remarks"></a>備註

如果其他使用者登入電腦並**安靜**命令未指定，會顯示提示，以確認應重新啟動電腦。

## <a name="BKMK_Examples"></a>範例

下列範例說明如何利用**重新啟動**命令。
```
bdehdcfg -target default -restart
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)
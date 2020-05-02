---
title: bdehdcfg 重新開機
description: Bdehdcfg restart 命令的參考主題，它會告訴 bdehdcfg，電腦應該在磁片磁碟機準備結束後重新開機。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 684a6a24fe78c0a23ba954981121c7bd99ac56fb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718629"
---
# <a name="bdehdcfg-restart"></a>bdehdcfg：重新開機

通知 bdehdcfg 命令列工具，電腦應該在磁片磁碟機準備結束後重新開機。 如果有其他使用者登入電腦，但未指定 [ **quiet** ] 命令，則會出現提示，確認電腦是否應重新開機。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -restart
```

#### <a name="parameters"></a>參數

此命令沒有任何其他參數。

## <a name="examples"></a>範例

若要使用**restart**命令：

```
bdehdcfg -target default -restart
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)

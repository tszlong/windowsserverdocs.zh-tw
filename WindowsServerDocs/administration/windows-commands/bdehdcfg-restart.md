---
title: bdehdcfg restart
description: Bdehdcfg 重新開機命令的參考文章，它會告訴 bdehdcfg，電腦應該在磁片磁碟機準備結束後重新開機。
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 67d2a3dd7b4304c26543840d6681b4ec6e655651
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895094"
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

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)

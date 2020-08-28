---
title: bdehdcfg restart
description: Bdehdcfg restart 命令的參考文章，此命令會告知 bdehdcfg 電腦在磁片磁碟機準備結束後應重新開機。
ms.topic: reference
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8db8ede29d5cecfdbe29031c21f86c5e2b8fc8c1
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031486"
---
# <a name="bdehdcfg-restart"></a>bdehdcfg：重新開機

通知 bdehdcfg 命令列工具，電腦必須在磁片磁碟機準備結束後重新開機。 如果有其他使用者登入電腦，但未指定 [無訊息 **] 命令，** 則會出現提示以確認電腦是否應該重新開機。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -restart
```

#### <a name="parameters"></a>參數

此命令沒有其他參數。

## <a name="examples"></a>範例

若要使用 **restart** 命令：

```
bdehdcfg -target default -restart
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)

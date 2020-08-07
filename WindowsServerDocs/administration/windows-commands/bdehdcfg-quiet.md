---
title: bdehdcfg quiet
description: Bdehdcfg quiet 命令的參考文章，告訴 bdehdcfg 不顯示所有動作和錯誤。
ms.topic: article
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7ddd6e2cb63b6af0ea0c50b5260436c184ee6aa6
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895086"
---
# <a name="bdehdcfg-quiet"></a>bdehdcfg： quiet

通知 bdehdcfg 命令列工具，所有動作和錯誤都不會顯示在命令列介面中。 在磁片磁碟機準備期間顯示的任何 [是/否] (Y/N) 提示都會假設有 [是] 答案。 若要查看磁片磁碟機準備期間發生的任何錯誤，請參閱**DrivePreparationTool**事件提供者底下的系統事件記錄檔。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -quiet
```

#### <a name="parameters"></a>參數

此命令沒有任何其他參數。

## <a name="examples"></a>範例

若要使用**quiet**命令：

```
bdehdcfg -target default -quiet
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)

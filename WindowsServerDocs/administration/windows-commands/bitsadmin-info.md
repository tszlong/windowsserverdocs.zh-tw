---
title: bitsadmin info
description: Bitsadmin info 命令的參考文章，它會顯示指定作業的摘要資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b9a284ee1e0ab8501f0fb6bc3417ca399996a08
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926551"
---
# <a name="bitsadmin-info"></a>bitsadmin info

顯示指定之作業的摘要資訊。

## <a name="syntax"></a>語法

```
bitsadmin /info <job> [/verbose]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| /verbose | 選擇性。 提供有關每項作業的詳細資訊。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的相關資訊：

```
bitsadmin /info myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin info](bitsadmin-info.md)

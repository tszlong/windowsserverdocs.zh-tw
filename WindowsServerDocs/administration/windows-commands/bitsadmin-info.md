---
title: bitsadmin info
description: Bitsadmin info 命令的參考文章，此命令會顯示指定工作的摘要資訊。
ms.topic: reference
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 815fdc719d584f7d25f88705056e4d5c0c3405aa
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631543"
---
# <a name="bitsadmin-info"></a>bitsadmin info

顯示指定工作的摘要資訊。

## <a name="syntax"></a>語法

```
bitsadmin /info <job> [/verbose]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| /verbose | 選擇性。 提供每項作業的詳細資訊。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的相關資訊：

```
bitsadmin /info myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin info](bitsadmin-info.md)

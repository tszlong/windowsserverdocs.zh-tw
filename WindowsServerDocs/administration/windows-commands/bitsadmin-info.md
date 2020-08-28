---
title: bitsadmin info
description: Bitsadmin info 命令的參考文章，此命令會顯示指定工作的摘要資訊。
ms.topic: reference
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a909655215d73b1fd197155810b980d5aaa04eab
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028566"
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

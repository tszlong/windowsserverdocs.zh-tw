---
title: unlodctr
description: Unlodctr 命令的參考文章，此命令會從系統登錄移除效能計數器名稱以及服務或設備磁碟機的解說文字。
ms.topic: reference
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3983f98df0de6a8048f78b6ebdb16cccafea8da4
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156641"
---
# <a name="unlodctr"></a>unlodctr

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從系統登錄移除 **效能計數器名稱** ，並說明服務或設備磁碟機的 **文字** 。

> [!WARNING]
> 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。

## <a name="syntax"></a>語法

```
unlodctr <drivername>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<drivername>` | 從 Windows Server 登錄移除 **效能計數器名稱** 設定，並說明驅動程式或服務的 **文字** `<drivername>` 。 如果您 `<drivername>` 包含空格，您必須使用引號括住文字，例如「驅動程式名稱」。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要移除目前的 **效能計數器名稱** ，並說明 Simple Mail Transfer PROTOCOL (SMTP) 服務的 **文字** ，請輸入：

```
unlodctr SMTPSVC
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

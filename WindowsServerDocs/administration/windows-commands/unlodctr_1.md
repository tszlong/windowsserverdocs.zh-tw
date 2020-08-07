---
title: unlodctr
description: Unlodctr 的參考文章，其會從系統登錄移除效能計數器名稱，並說明服務或設備磁碟機的文字。
ms.topic: article
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3c867a4634024527066c329f408a210e97718d1c
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87897048"
---
# <a name="unlodctr"></a>unlodctr

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從系統登錄移除**效能計數器**名稱，並**說明**服務或設備磁碟機的文字。

## <a name="syntax"></a>語法
```
Unlodctr <DriverName>
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|\<DriverName>|從 Windows Server 2003 登錄中移除效能計數器名稱設定，並說明驅動程式或服務的文字 <DriverName> 。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
> [!WARNING]
> 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。

如果您提供的資訊包含空格，請使用引號括住文字 (例如， <DriverName>) 。

## <a name="examples"></a>範例
若要移除目前的效能登錄設定和計數器說明 Simple Mail Transfer Protocol (SMTP) 服務的文字：
```
unlodctr SMTPSVC
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)


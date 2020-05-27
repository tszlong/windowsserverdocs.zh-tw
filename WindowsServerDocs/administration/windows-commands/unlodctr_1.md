---
title: unlodctr
description: Unlodctr 的參考主題，其會從系統登錄移除效能計數器名稱，並說明服務或設備磁碟機的文字。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8551b6fc76984b06f28bdda92dcd63791721ec90
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821288"
---
# <a name="unlodctr"></a>unlodctr

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從系統登錄移除**效能計數器**名稱，並**說明**服務或設備磁碟機的文字。

## <a name="syntax"></a>語法
```
Unlodctr <DriverName>
```
#### <a name="parameters"></a>參數
|參數|說明|
|-------|--------|
|\<DriverName>|從 Windows Server 2003 登錄中移除效能計數器名稱設定，並說明驅動程式或服務的文字 <DriverName> 。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
> [!WARNING]
> 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。

如果您提供的資訊包含空格，請使用引號括住文字（例如， <DriverName> ）。

## <a name="examples"></a>範例
若要移除「簡易郵件傳送通訊協定（SMTP）」服務的目前效能登錄設定和計數器解說文字：
```
unlodctr SMTPSVC
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)


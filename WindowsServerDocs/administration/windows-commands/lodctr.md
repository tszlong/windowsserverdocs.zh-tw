---
title: lodctr
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a849abd-6b31-4833-bc8a-306c05eca29a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ea48067bd78d260824da7d091f94957b51b768d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852219"
---
# <a name="lodctr"></a>lodctr

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

可讓您註冊，或將效能計數器名稱和登錄設定儲存於檔案並將指定受信任的服務。
## <a name="syntax"></a>語法
```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|<filename>|登錄效能計數器名稱設定和初始設定檔案的檔名中提供的說明文字。|
|/s:<filename>|儲存效能計數器登錄設定，然後說明文字檔案<filename>。|
|/r|從目前的登錄設定和快取的效能與登錄相關的檔案會還原計數器登錄設定和解說文字。<br /><br />此選項是僅適用於 Windows Server 2003 作業系統。|
|/r:<filename>|還原效能計數器登錄設定，然後從檔案來解釋文字<filename>。 **警告：** 如果您使用**lodctr /r**命令中，您將會覆寫所有效能計數器登錄設定，並解說文字，並取代成指定的檔案中定義的組態。|
|/t:<servicename>|指出該服務<servicename>受到信任。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
如果您提供的資訊包含空格，請使用引號括住的文字 (例如，"<filename>」)。
## <a name="BKMK_Examples"></a>範例
儲存目前的效能登錄設定和計數器檔案的說明文字**效能 backup1.txt**:
```
lodctr /s:"perf backup1.txt"
```
## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)


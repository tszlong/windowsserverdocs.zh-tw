---
title: bitsadmin setnotifycmdline
description: 適用於 Windows 命令主題 * * *-bitsadmin setnotifycmdlineSets 傳輸資料的作業完成時，或工作進入狀態時將執行的命令列命令。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1cea4e99cbaaf3881c6f436bdb932090ad6b006
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859069"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

設定在作業完成的傳輸資料時，或工作進入狀態時將執行的命令列命令。

**1.2 及更早版本的位元**: 未支援。

## <a name="syntax"></a>語法

```
bitsadmin /SetNotifyCmdLine <Job> <ProgramName> [ProgramParameters]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|ProgramName|當作業完成時要執行的命令名稱。|
|ProgramParameters|您想要傳遞至參數*ProgramName*。|

## <a name="remarks"></a>備註

您可以指定 NULL *ProgramName*並*ProgramParameters*。 如果*ProgramName*為 NULL，就*ProgramParameters*必須是 NULL。

> [!IMPORTANT]
> 如果*ProgramParameters*不是 NULL，則在第一個參數*ProgramParameters*必須符合*ProgramName*。

## <a name="BKMK_examples"></a>範例

下列範例會設定時，作業命名為執行 「 記事本 」 服務所使用的命令列命令*myDownloadJob*完成。
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe NULL
```
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe "notepad c:\eula.txt"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
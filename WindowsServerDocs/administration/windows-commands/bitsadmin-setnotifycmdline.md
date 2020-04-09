---
title: bitsadmin setnotifycmdline
description: 適用于 bitsadmin setnotifycmdline 的 Windows 命令主題，會設定當作業完成傳輸資料或工作進入狀態時，將會執行的命令列命令。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 761a7003e44e8dc15cb2dd2f1ce5a1a23be53286
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849331"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

設定當作業完成傳輸資料或工作進入狀態時，將會執行的命令列命令。

**BITS 1.2 和更早版本**：不支援。

## <a name="syntax"></a>語法

```
bitsadmin /SetNotifyCmdLine <Job> <ProgramName> [ProgramParameters]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|ProgramName|作業完成時要執行的命令名稱。|
|ProgramParameters|您想要傳遞給*ProgramName*的參數。|

## <a name="remarks"></a>備註

您可以為*ProgramName*和*ProgramParameters*指定 Null。 如果*ProgramName*為 null，則*PROGRAMPARAMETERS*必須是 null。

> [!IMPORTANT]
> 如果*ProgramParameters*不是 Null，則*ProgramParameters*中的第一個參數必須符合*ProgramName*。

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會設定當名為*myDownloadJob*的作業完成時，服務用來執行「記事本」的命令列命令。
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe NULL
```
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe notepad c:\eula.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
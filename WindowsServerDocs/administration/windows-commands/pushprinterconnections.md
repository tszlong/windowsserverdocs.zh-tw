---
title: pushprinterconnections
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c571d3adffd0e6a28f63f6d821b2524dc055aa9a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873719"
---
# <a name="pushprinterconnections"></a>pushprinterconnections



讀取群組原則部署印表機連線設定，並部署/移除印表機連線所需。

## <a name="syntax"></a>語法

```
pushprinterconnections <-log> <-?>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|<-log>|寫入每個使用者偵錯記錄檔 %temp 或寫入每個 windir%\temp 機器偵錯記錄。|
|<-?>|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

這個公用程式是用於電腦啟動或使用者登入指令碼，並且不應該從命令列執行。

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [使用群組原則部署印表機](https://go.microsoft.com/fwlink/?LinkId=230627)
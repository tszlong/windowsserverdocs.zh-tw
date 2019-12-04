---
title: wmic
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 76397c72-d06f-4cea-88cf-c7603315a983
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f5096ab82ebbd01cb4f3a7dc0cf0b15e4b9fae8e
ms.sourcegitcommit: effbc183bf4b370905d95c975626c1ccde057401
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2019
ms.locfileid: "74781325"
---
# <a name="wmic"></a>wmic



在互動式命令 shell 內顯示 WMI 資訊。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
wmic </parameter>
```

## <a name="sub-commands"></a>子命令

下列子命令隨時都可供使用：

|子命令|說明|
|-----------|-----------|
|class|從 WMIC 的預設別名模式 esc，以直接存取 WMI 架構中的類別。|
|path|從 WMIC 的預設別名模式 esc，以直接存取 WMI 架構中的實例。|
|內容|顯示所有全域參數的目前值。|
|[quit \| exit]|結束 WMIC 命令 shell。|

## <a name="BKMK_examples"></a>典型

若要顯示所有全域參數的目前值，請輸入：
```
wmic context
```
輸出類似下列顯示：
```
NAMESPACE    : root\cimv2
ROLE         : root\cli
NODE(S)      : BOBENTERPRISE
IMPLEVEL     : IMPERSONATE
[AUTHORITY   : N/A]
AUTHLEVEL    : PKTPRIVACY
LOCALE       : ms_409
PRIVILEGES   : ENABLE
TRACE        : OFF
RECORD       : N/A
INTERACTIVE  : OFF
FAILFAST     : OFF
OUTPUT       : STDOUT
APPEND       : STDOUT
USER         : N/A
AGGREGATE    : ON
```
若要將命令列使用的語言識別項變更為英文（地區設定識別碼409），請輸入：
```
wmic /locale:ms_409
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

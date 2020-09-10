---
title: wmic
description: 適用于 wmic 的參考文章，會在互動式命令 shell 中顯示 WMI 資訊。
ms.topic: reference
ms.assetid: 76397c72-d06f-4cea-88cf-c7603315a983
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d79f00798b3901d1b8cc44d313824130b1c8fb62
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638863"
---
# <a name="wmic"></a>wmic



顯示互動式命令 shell 內的 WMI 資訊。



## <a name="syntax"></a>語法

```
wmic </parameter>
```

## <a name="sub-commands"></a>子命令

您可以隨時使用下列子命令：

|子命令|描述|
|-----------|-----------|
|Class - 類別|從預設的 WMIC 別名模式中的 esc 來直接存取 WMI 架構中的類別。|
|path|從預設的 WMIC 別名模式中的 esc 來直接存取 WMI 架構中的實例。|
|內容|顯示所有全域參數的目前值。|
|[quit \| exit]|退出 WMIC 命令 shell。|

## <a name="examples"></a>範例

若要顯示所有全域參數的目前值，請輸入：
```
wmic context
```
如下所示的輸出：
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
若要將命令列所使用的語言識別項變更為英文 (地區設定識別碼 409) ，請輸入：
```
wmic /locale:ms_409
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

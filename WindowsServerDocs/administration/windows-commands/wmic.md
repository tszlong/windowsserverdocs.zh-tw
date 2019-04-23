---
title: wmic
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6c68866fbe0c8f5b16dae77e2121331f06cdc726
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885839"
---
# <a name="wmic"></a>wmic



顯示 WMI 互動式命令 shell 內的資訊。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
command </parameter>
```

## <a name="sub-commands"></a>子命令

下列子命令隨時都可以使用：

|子命令|描述|
|-----------|-----------|
|class|從 WMIC 直接存取 WMI 結構描述中的類別的預設別名模式的逸出。|
|path|從 WMIC 直接存取 WMI 架構中的執行個體的預設別名模式的逸出。|
|內容|顯示所有的通用參數的目前值。|
|[結束\|結束]|結束 WMIC 命令殼層。|

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|</parameter>|\<開頭為精確的描述，動詞命令。 >|
|</param2>|\<另一個的精簡描述開頭為動詞。 >|


## <a name="BKMK_examples"></a>範例

若要顯示所有的通用參數目前的值，請輸入：
```
wmic context
```
輸出會顯示下列類似：
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
若要變更語言識別碼使用英文 （地區設定識別碼 409），在命令列型別：
```
wmic /locale:ms_409
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
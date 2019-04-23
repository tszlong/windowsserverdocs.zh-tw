---
title: bitsadmin util 和版本
description: 適用於 Windows 命令主題**bitsadmin util 和版本**-顯示 BITS 服務的版本。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e768ec5ae43fc17c480b9deede698cca01c6291
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882869"
---
# <a name="bitsadmin-util-and-version"></a>bitsadmin util 和版本

顯示 BITS 服務 (例如 2.0) 的版本。

**1.5 和更早版本的 BITSAdmin**: 未支援。

## <a name="syntax"></a>語法

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>備註

**Verbose**參數會執行下列：
-   顯示每個 BITS 相關的 DLL 檔案版本
-   確認可以啟動 BITS 服務
-   顯示位元群組原則 」 值 (只在 Windows Vista)

## <a name="BKMK_examples"></a>範例

下列範例的 BITS 服務版本。
```
C:\>bitsadmin /Util /Version
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
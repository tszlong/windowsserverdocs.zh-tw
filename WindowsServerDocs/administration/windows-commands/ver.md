---
title: ver
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 384a5e8adb6c8304033f7dc645184ff2b674ae39
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887169"
---
# <a name="ver"></a>ver



顯示作業系統版本號碼。

在 Windows 命令提示字元 (Cmd.exe)，但在 PowerShell 中不支援這個命令。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
ver
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_examples"></a>範例

若要取得作業系統的版本號碼，從命令殼層 (cmd.exe)，請輸入：

```
ver
```

[Ver] 命令在 PowerShell 中無法運作。 若要從 PowerShell 中取得的 OS 版本，請輸入：

```powershell
$PSVersionTable.BuildVersion
````


#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

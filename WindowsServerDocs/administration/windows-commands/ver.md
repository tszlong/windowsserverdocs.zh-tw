---
title: ver
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e48b3b1061edf793c88693b3353753c6a4cedcfc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362719"
---
# <a name="ver"></a>ver



顯示作業系統版本號碼。

Windows 命令提示字元（Cmd.exe）中支援此命令，但在 PowerShell 中則否。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
ver
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_examples"></a>典型

若要從命令 shell （cmd.exe）取得作業系統的版本號碼，請輸入：

```
ver
```

Ver 命令無法在 PowerShell 中運作。 若要從 PowerShell 取得 OS 版本，請輸入：

```powershell
$PSVersionTable.BuildVersion
````


#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

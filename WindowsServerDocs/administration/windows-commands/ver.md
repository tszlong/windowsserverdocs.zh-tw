---
title: ver
description: 適用于 ver 的 Windows 命令主題，它會顯示作業系統版本號碼。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0d0676dcfa6546e4bbf74c4c58a24f51744d00f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830191"
---
# <a name="ver"></a>ver



顯示作業系統版本號碼。

Windows 命令提示字元（Cmd.exe）中支援此命令，但在 PowerShell 中則否。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
ver
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/?|在命令提示字元顯示說明。|

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要從命令 shell （cmd.exe）取得作業系統的版本號碼，請輸入：

```
ver
```

Ver 命令無法在 PowerShell 中運作。 若要從 PowerShell 取得 OS 版本，請輸入：

```powershell
$PSVersionTable.BuildVersion
````


## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

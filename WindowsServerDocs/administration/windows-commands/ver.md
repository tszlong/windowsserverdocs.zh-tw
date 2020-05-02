---
title: ver
description: Ver 的參考主題，會顯示作業系統版本號碼。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7050dddda6cc27c50980f2e44f40e1f682c1d375
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720306"
---
# <a name="ver"></a>ver



顯示作業系統版本號碼。

Windows 命令提示字元（Cmd.exe）中支援此命令，但在 PowerShell 中則否。



## <a name="syntax"></a>語法

```
ver
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/?|在命令提示字元顯示說明。|

## <a name="examples"></a>範例

若要從命令 shell （cmd.exe）取得作業系統的版本號碼，請輸入：

```
ver
```

Ver 命令無法在 PowerShell 中運作。 若要從 PowerShell 取得 OS 版本，請輸入：

```powershell
$PSVersionTable.BuildVersion
````


## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

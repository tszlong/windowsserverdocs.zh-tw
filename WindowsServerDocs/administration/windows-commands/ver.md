---
title: ver
description: Ver 的參考文章，會顯示作業系統版本號碼。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd9b40fa526c2917b6cdcbc8d54da510eb40bc53
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931346"
---
# <a name="ver"></a>ver



顯示作業系統版本號碼。

Windows 命令提示字元（Cmd.exe）中支援此命令，但在 PowerShell 中則否。



## <a name="syntax"></a>語法

```
ver
```

### <a name="parameters"></a>參數

|參數|說明|
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


## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

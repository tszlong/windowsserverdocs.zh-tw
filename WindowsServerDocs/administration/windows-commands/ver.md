---
title: ver
description: 適用于 ver 的參考文章，會顯示作業系統版本號碼。
ms.topic: reference
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 543aceee52be60b6dc90509c326ba1172b2d1ca6
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023042"
---
# <a name="ver"></a>ver



顯示作業系統版本號碼。

Windows 命令提示字元 ( # A0) 中支援這個命令，但在 PowerShell 中則不支援。



## <a name="syntax"></a>語法

```
ver
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/?|在命令提示字元顯示說明。|

## <a name="examples"></a>範例

若要從命令 shell ( # A0) 取得作業系統的版本號碼，請輸入：

```
ver
```

Ver 命令在 PowerShell 中無法運作。 若要從 PowerShell 取得作業系統版本，請輸入：

```powershell
$PSVersionTable.BuildVersion
````


## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

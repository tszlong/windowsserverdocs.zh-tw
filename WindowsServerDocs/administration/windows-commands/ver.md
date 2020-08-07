---
title: ver
description: Ver 的參考文章，會顯示作業系統版本號碼。
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de080395c2e26f03371e0b27609238b66d7317f5
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891889"
---
# <a name="ver"></a>ver



顯示作業系統版本號碼。

Windows 命令提示字元 ( # A0) ，但 PowerShell 中不支援此命令。



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

Ver 命令無法在 PowerShell 中運作。 若要從 PowerShell 取得 OS 版本，請輸入：

```powershell
$PSVersionTable.BuildVersion
````


## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

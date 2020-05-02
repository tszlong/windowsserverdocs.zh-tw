---
title: help
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c65b5ac3-711a-4368-95b8-ba82e2d00713
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d3a16c2534934a7bc8126b0a775ec7aa08462b3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724918"
---
# <a name="help"></a>help



提供有關系統命令的線上資訊（也就是非網路命令）。 如果在沒有**參數的情況**下使用，說明會列出並簡短地描述每個系統命令。



## <a name="syntax"></a>語法

```
help [<Command>] 
[<Command>] /?
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<命令>|指定您想要其相關資訊的命令名稱。|

## <a name="examples"></a>範例

若要查看**robocopy**命令的相關資訊，請輸入下列其中一項：
```
help robocopy
robocopy /? 
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
---
title: bitsadmin getowner
description: Bitsadmin getowner 命令的參考主題，它會抓取指定之作業的擁有者。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a345ea47232f1f4d6340e1341747c9dad92382b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717712"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

顯示指定作業之擁有者的顯示名稱或 GUID。

## <a name="syntax"></a>語法

```
bitsadmin /getowner <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要顯示名為*myDownloadJob*之作業的擁有者：

```
bitsadmin /getowner myDownloadJob
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

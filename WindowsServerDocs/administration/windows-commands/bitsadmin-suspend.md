---
title: bitsadmin suspend
description: Bitsadmin 暫止命令的參考主題，它會暫停指定的工作。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8117cf9f4286994847e53dca8065da6821d47c5d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720454"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

暫停指定的工作。 如果您不小心暫停工作，可以使用[bitsadmin resume](bitsadmin-resume.md)參數重新開機作業。

## <a name="syntax"></a>語法

```
bitsadmin /suspend <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ---------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="example"></a>範例

若要暫停名為*myDownloadJob*的作業：


```
bitsadmin /suspend myDownloadJob
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin resume 命令](bitsadmin-resume.md)

- [bitsadmin 命令](bitsadmin.md)

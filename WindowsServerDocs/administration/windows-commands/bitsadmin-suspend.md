---
title: bitsadmin suspend
description: '**Bitsadmin 暫停**的 Windows 命令主題，這會暫停指定的工作。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42ed83d4dbf8c3d982c5c186b440cf17997903c9
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123158"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> 適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

暫停指定的工作。 如果您不小心暫停工作，可以使用[bitsadmin resume](bitsadmin-resume.md)參數重新開機作業。

## <a name="syntax"></a>語法

```
bitsadmin /suspend <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ---------- |
| Job | 作業的顯示名稱或 GUID。 |

## <a name="example"></a>範例

下列範例會暫停名為*myDownloadJob*的作業。


```
C:\>bitsadmin /suspend myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

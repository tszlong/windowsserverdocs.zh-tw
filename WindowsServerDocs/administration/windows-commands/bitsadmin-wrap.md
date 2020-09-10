---
title: bitsadmin wrap
description: Bitsadmin wrap 命令的參考文章，此命令會將任何行的輸出文字包裝在命令視窗的最右邊邊緣到下一行。
ms.topic: reference
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 24fa50e153d52ec74ab728a132aadd1b95c7a9da
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630387"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將任何行的輸出文字換行至命令視窗的最右邊緣到下一行。 您必須在任何其他參數之前指定這個參數。

根據預設，所有參數（ [bitsadmin 監視器](bitsadmin-monitor.md) 參數除外）都會包裝輸出文字。

## <a name="syntax"></a>語法

```
bitsadmin /wrap <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ---------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob* 之作業的資訊，並包裝輸出文字：

```
bitsadmin /wrap /info myDownloadJob /verbose
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

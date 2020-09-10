---
title: bitsadmin nowrap
description: Bitsadmin nowrap 命令的參考文章，它會截斷任何超出命令視窗最右邊邊緣的輸出文字行。
ms.topic: reference
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2e5724f3dedb808898cd070026e2e4a944d4731b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631419"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

將任何超出命令視窗最右邊邊緣的輸出文字行截斷。 根據預設，所有參數（ **監視器** 參數除外）都會包裝輸出。 在其他參數之前指定 **nowrap** 參數。

## <a name="syntax"></a>語法

```
bitsadmin /nowrap
```

## <a name="examples"></a>範例

若要在未包裝輸出的情況下，取得名為 *myDownloadJob* 之作業的狀態：

```
bitsadmin /nowrap /getstate myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

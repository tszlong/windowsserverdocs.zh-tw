---
title: bitsadmin setpriority
description: Bitsadmin setpriority 命令的參考文章，此命令會設定指定工作的優先順序。
ms.topic: reference
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 25bf7026ceef21fb37824ce99f56389941426b79
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630719"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority

設定指定之工作的優先順序。

## <a name="syntax"></a>語法

```
bitsadmin /setpriority <job> <priority>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| priority | 設定工作的優先順序，包括：<ul><li>FOREGROUND</li><li>HIGH</li><li>NORMAL</li><li>LOW</li></ul> |

## <a name="examples"></a>範例

若要將名為 *myDownloadJob* 的作業優先權設定為 normal：

```
bitsadmin /setpriority myDownloadJob NORMAL
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)

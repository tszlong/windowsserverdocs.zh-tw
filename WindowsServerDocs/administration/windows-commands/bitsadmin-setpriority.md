---
title: bitsadmin setpriority
description: Bitsadmin setpriority 命令的參考文章，此命令會設定指定工作的優先順序。
ms.topic: reference
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b1fe6a2b3981697a4a8c287fe4fb49c31a4f4244
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028466"
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

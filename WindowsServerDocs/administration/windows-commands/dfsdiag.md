---
title: dfsdiag
description: Dfsdiag 命令的參考主題，其提供 DFS 命名空間的診斷資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7e9e0de18b48a4233b950ad6aa8f1e450a99da62
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992831"
---
# <a name="dfsdiag"></a>dfsdiag

提供 DFS 命名空間的診斷資訊。

## <a name="syntax"></a>語法

```
dfsdiag /testdcs [/domain:<domain name>]
dfsdiag /testsites </machine:<server name>| /DFSPath:<namespace root or DFS folder> [/recurse]> [/full]
dfsdiag /testdfsconfig /DFSRoot:<namespace>
dfsdiag /testdfsintegrity /DFSRoot:<DFS root path> [/recurse] [/full]
dfsdiag /testreferral /DFSpath:<DFS path to get referrals> [/full]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| [dfsdiag testdcs](dfsdiag-testdcs.md) | 檢查網域控制站設定。 |
| [dfsdiag testsites](dfsdiag-testsites.md) | 檢查網站關聯。 |
| [dfsdiag testdfsconfig](dfsdiag-testdfsconfig.md) | 檢查 DFS 命名空間設定。 |
| [dfsdiag testdfsintegrity](dfsdiag-testdfsintegrity.md) | 檢查 DFS 命名空間的完整性。 |
| [dfsdiag testreferral](dfsdiag-testreferral.md) | 檢查參照回應。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

---
title: dfsdiag TestDFSIntegrity
description: '**Dfsdiag TestDFSIntegrity**的參考主題，它會檢查分散式檔案系統（DFS）命名空間的完整性。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21aa6ef3d7d4a7b4a9c64fc51aec77f49f1e0a0c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719581"
---
# <a name="dfsdiag-testdfsintegrity"></a>dfsdiag TestDFSIntegrity

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由執行下列測試，檢查分散式檔案系統（DFS）命名空間的完整性：

- 檢查網域控制站之間的 DFS 中繼資料損毀或不一致。

- 驗證存取型列舉的設定，以確保 DFS 中繼資料與命名空間伺服器共用之間的設定一致。

- 偵測重迭的 DFS 資料夾（連結）、重複的資料夾，以及具有重迭資料夾目標的資料夾。

## <a name="syntax"></a>語法

```
dfsdiag /TestDFSIntegrity /DFSRoot: <DFS root path> [/Recurse] [/Full]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
|-------|--------|
| /DFSRoot:`<DFS root path>`| 要診斷的 DFS 命名空間。 |
| /Recurse | 執行測試，包括命名空間連結。 |
| /Full | 在所有資料夾目標上驗證共用和 NTFS Acl 和用戶端設定的一致性。 它也會驗證 online 屬性是否已設定。 |

## <a name="examples"></a>範例

```
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full
```

## <a name="additional-references"></a>其他參考

-   - [命令列語法關鍵](command-line-syntax-key.md)

-   [dfsdiag](dfsdiag.md)



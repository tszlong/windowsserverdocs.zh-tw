---
title: dfsdiag testdfsintegrity
description: Dfsdiag testdfsintegrity 命令的參考文章，此命令會檢查分散式檔案系統 (DFS) 命名空間的完整性。
ms.topic: reference
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7bcfbe7f35965322a347651133a90e6806a5bb95
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028416"
---
# <a name="dfsdiag-testdfsintegrity"></a>dfsdiag testdfsintegrity

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由執行下列測試，檢查分散式檔案系統 (DFS) 命名空間的完整性：

- 檢查是否有 DFS 中繼資料損毀或網域控制站之間的不一致。

- 驗證以存取為基礎之列舉的設定，以確保 DFS 中繼資料與命名空間伺服器共用之間的設定一致。

- 偵測到重迭的 DFS 資料夾 (連結) 、重複的資料夾以及具有重迭資料夾目標的資料夾。

## <a name="syntax"></a>語法

```
dfsdiag /testdfsintegrity /DFSroot: <DFS root path> [/recurse] [/full]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /DFSroot: `<DFS root path>` | 要診斷的 DFS 命名空間。 |
| /recurse | 執行測試，包括任何命名空間連結。 |
| /full | 確認共用和 NTFS Acl 的一致性，以及所有資料夾目標上的用戶端設定。 它也會確認 online 屬性已設定。 |

## <a name="examples"></a>範例

若要確認 *com\MyNamespace*中 (DFS) 命名空間的完整性與分散式檔案系統一致性，包括任何連結，請輸入：

```
dfsdiag /testdfsintegrity /DFSRoot:\contoso.com\MyNamespace /recurse /full
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [dfsdiag 命令](dfsdiag.md)

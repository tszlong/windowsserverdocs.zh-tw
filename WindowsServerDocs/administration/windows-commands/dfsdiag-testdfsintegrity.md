---
title: dfsdiag TestDFSIntegrity
description: 適用于**Dfsdiag TestDFSIntegrity**的 Windows 命令主題，它會檢查分散式檔案系統（DFS）命名空間的完整性。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 714b79369898338a4e4a6e4fad8487709ab4fc60
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846271"
---
# <a name="dfsdiag-testdfsintegrity"></a>dfsdiag TestDFSIntegrity

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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
| /DFSRoot： `<DFS root path>`| 要診斷的 DFS 命名空間。 |
| /Recurse | 執行測試，包括命名空間連結。 |
| /Full | 在所有資料夾目標上驗證共用和 NTFS Acl 和用戶端設定的一致性。 它也會驗證 online 屬性是否已設定。 |

## <a name="examples"></a><a name=BKMK_Examples></a>典型

```
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full
```

## <a name="additional-references"></a>其他參考資料

-   - [命令列語法關鍵](command-line-syntax-key.md)

-   [dfsdiag](dfsdiag.md)



---
title: dfsdiag testdfsconfig
description: Dfsdiag testdfsconfig 的參考文章，會檢查分散式檔案系統 (DFS) 命名空間的設定。
ms.topic: reference
ms.assetid: 106aeeb9-ea79-4e6e-829c-eca06309bab2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a95e5df19bfab4a8724d755b4495dc90bcf5f93a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634145"
---
# <a name="dfsdiag-testdfsconfig"></a>dfsdiag testdfsconfig

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由執行下列動作來檢查分散式檔案系統 (DFS) 命名空間的設定：

- 確認 DFS 命名空間服務正在執行，而且在所有命名空間伺服器上的啟動類型都設定為 [ **自動** ]。

- 確認 DFS 登錄設定在命名空間伺服器之間是一致的。

- 驗證叢集命名空間伺服器上的下列相依性：

  - 網路名稱資源的命名空間根資源相依性。

  - IP 位址資源上的網路名稱資源相依性。

  - 實體磁片資源的命名空間根資源相依性。

## <a name="syntax"></a>語法

```
dfsdiag /testdfsconfig /DFSroot:<namespace>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /DFSroot:`<namespace>` | 命名空間 (DFS 根目錄) 以進行診斷。 |

## <a name="examples"></a>範例

若要確認 *com\MyNamespace*中 (DFS) 命名空間分散式檔案系統的設定，請輸入：

```
dfsdiag /testdfsconfig /DFSroot:\contoso.com\MyNamespace
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [dfsdiag 命令](dfsdiag.md)

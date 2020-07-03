---
title: dfsdiag testdfsconfig
description: Dfsdiag testdfsconfig 的參考文章，它會檢查分散式檔案系統（DFS）命名空間的設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 106aeeb9-ea79-4e6e-829c-eca06309bab2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3387b661f454cff089f76f7c9c0d1abe59387010
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930640"
---
# <a name="dfsdiag-testdfsconfig"></a>dfsdiag testdfsconfig

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由執行下列動作，檢查分散式檔案系統（DFS）命名空間的設定：

- 確認 DFS 命名空間服務正在執行，且其啟動類型已在所有命名空間伺服器上設定為 [**自動**]。

- 確認 DFS 登錄設定在命名空間伺服器之間是一致的。

- 驗證叢集命名空間伺服器上的下列相依性：

  - 網路名稱資源的命名空間根資源相依性。

  - IP 位址資源的網路名稱資源相依性。

  - 實體磁片資源上的命名空間根資源相依性。

## <a name="syntax"></a>語法

```
dfsdiag /testdfsconfig /DFSroot:<namespace>
```

#### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| /DFSroot:`<namespace>` | 要診斷的命名空間（DFS 根）。 |

## <a name="examples"></a>範例

若要確認*com\MyNamespace*中的分散式檔案系統（DFS）命名空間的設定，請輸入：

```
dfsdiag /testdfsconfig /DFSroot:\contoso.com\MyNamespace
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [dfsdiag 命令](dfsdiag.md)

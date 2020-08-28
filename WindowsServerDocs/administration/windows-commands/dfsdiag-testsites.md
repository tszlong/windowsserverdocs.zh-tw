---
title: dfsdiag testsites
description: Dfsdiag testsites 的參考文章，藉由確認作為命名空間伺服器或資料夾的伺服器 (連結) 目標在所有網域控制站上具有相同的網站關聯，檢查 active directory 網域服務的設定 (AD DS) 網站。
ms.topic: reference
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f024059f8b67b783da6a9ce574695aac7d7829b3
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034156"
---
# <a name="dfsdiag-testsites"></a>dfsdiag testsites

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

確認 active directory 網域服務的設定 (AD DS) 網站，方法是確認作為命名空間伺服器或資料夾的伺服器 (連結) 目標在所有網域控制站上都有相同的網站關聯。

## <a name="syntax"></a>語法

```
dfsdiag /testsites </machine:<server name>| /DFSpath:<namespace root or DFS folder> [/recurse]> [/full]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `/machine:<server name>` | 要在其上驗證網站關聯的伺服器名稱。 |
| `/DFSpath:<namespace root or DFS folder>` | 命名空間根或分散式檔案系統 (DFS) 資料夾 (連結) 與要驗證網站關聯的目標。 |
| /recurse | 列舉並驗證所指定命名空間根目錄下所有資料夾目標的網站關聯。 |
| /full | 確認 AD DS 和伺服器的登錄包含相同的網站關聯資訊。 |

## <a name="examples"></a>範例

若要檢查 *machine\MyServer*上的網站關聯，請輸入：

```
dfsdiag /testsites /machine:MyServer
```

若要檢查分散式檔案系統 (DFS) 資料夾來確認網站關聯，以及確認伺服器的 AD DS 和登錄包含相同的網站關聯資訊，請輸入：

```
dfsdiag /TestSites /DFSpath:\\contoso.com\namespace1\folder1 /full
```

若要檢查命名空間根來確認網站關聯，以及列舉和驗證指定命名空間根目錄下所有資料夾目標的網站關聯，以及驗證 AD DS 和伺服器的登錄是否包含相同的網站關聯資訊，請輸入：

```
dfsdiag /testsites /DFSpath:\\contoso.com\namespace2 /recurse /full
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [dfsdiag 命令](dfsdiag.md)

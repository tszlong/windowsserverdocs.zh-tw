---
title: dfsdiag testsites
description: Dfsdiag testsites 的參考主題，它會確認做為命名空間伺服器或資料夾（連結）目標的伺服器在所有網域控制站上都有相同的網站關聯，藉此檢查 active directory 網域服務（AD DS）網站的設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54eb7c7ec44d7cd4872960ca29cd3146b710f472
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992845"
---
# <a name="dfsdiag-testsites"></a>dfsdiag testsites

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

檢查 active directory 網域服務（AD DS）網站的設定，方法是確認作為命名空間伺服器或資料夾（連結）目標的伺服器在所有網域控制站上都有相同的網站關聯。

## <a name="syntax"></a>語法

```
dfsdiag /testsites </machine:<server name>| /DFSpath:<namespace root or DFS folder> [/recurse]> [/full]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `/machine:<server name>` | 要驗證其網站關聯的伺服器名稱。 |
| `/DFSpath:<namespace root or DFS folder>` | 命名空間根或分散式檔案系統（DFS）資料夾（連結），具有要驗證其網站關聯的目標。 |
| /recurse | 列舉並驗證指定之命名空間根目錄下所有資料夾目標的網站關聯。 |
| /full | 確認伺服器的 AD DS 和登錄包含相同的網站關聯資訊。 |

## <a name="examples"></a>範例

若要檢查*machine\MyServer*上的網站關聯，請輸入：

```
dfsdiag /testsites /machine:MyServer
```

若要檢查分散式檔案系統（DFS）資料夾來確認網站關聯，並確認伺服器的 AD DS 和登錄包含相同的網站關聯資訊，請輸入：

```
dfsdiag /TestSites /DFSpath:\\contoso.com\namespace1\folder1 /full
```

若要檢查命名空間根目錄以確認網站關聯，以及在指定的命名空間根目錄下列舉和驗證所有資料夾目標的網站關聯，並確認伺服器的 AD DS 和登錄包含相同的網站關聯資訊，請輸入：

```
dfsdiag /testsites /DFSpath:\\contoso.com\namespace2 /recurse /full
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [dfsdiag 命令](dfsdiag.md)

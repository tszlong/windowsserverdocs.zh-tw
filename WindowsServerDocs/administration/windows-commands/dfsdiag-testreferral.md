---
title: dfsdiag testreferral
description: Dfsdiag testreferral 命令的參考文章，此命令會檢查分散式檔案系統 (DFS) 參考。
ms.topic: reference
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 245238c83a9795dd4976836bae612f5e00a25a5b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034176"
---
# <a name="dfsdiag-testreferral"></a>dfsdiag testreferral

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由執行下列測試，檢查分散式檔案系統 (DFS) 參照：

- 如果您使用不含引數的 **DFSpath*** 參數，此命令會驗證參考清單是否包含所有信任的網域。

- 如果您指定網域，此命令會執行網域控制站的健康情況檢查 (`dfsdiag /testdcs`) 並測試本機主機的網站關聯和網域快取。

- 如果您指定網域和 \SYSvol 或 \NETLOGON，此命令會執行相同的網域控制站健康情況檢查，並檢查 SYSvol 或 NETLOGON 參考的 **存留時間 (TTL) ** 是否符合900秒的預設值。

- 如果您指定命名空間根目錄，此命令會執行相同的網域控制站健康情況檢查，以及執行 DFS 設定檢查 (`dfsdiag /testdfsconfig`) 以及命名空間完整性檢查 (`dfsdiag /testdfsintegrity`) 。

- 如果您指定 DFS 資料夾 (連結) ，此命令會執行相同的命名空間根健康情況檢查，以及驗證資料夾目標的網站設定 (dfsdiag/testsites) 以及驗證本機主機的網站關聯。

## <a name="syntax"></a>語法

```
dfsdiag /testreferral /DFSpath:<DFS path to get referrals> [/full]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /DFSpath:`<path to get referrals>` | 可以是下列其中一項：<ul><li>**空白：** 只測試受信任的網域。</li><li>`\\Domain:` 僅測試網域控制站的參考。</li><li>`\\Domain\SYSvol:` 只測試 SYSvol 的參考。</li><li>`\\Domain\NETLOGON:` 只測試 NETLOGON 參考。</li><li>`\\<domain or server>\<namespace root>:` 僅測試命名空間根參考。</li><li>`\\<domain or server>\<namespace root>\<DFS folder>:` 只測試 DFS 資料夾 (連結) 的參考。</li></ul> |
| /full | 只適用于網域和根參考。 在登錄和 active directory 網域服務 (AD DS) 之間，確認網站關聯資訊的一致性。 |

## <a name="examples"></a>範例

若要檢查 *com\MyNamespace*中 (DFS) 參照的分散式檔案系統，請輸入：

```
dfsdiag /testreferral /DFSpath:\\contoso.com\MyNamespace
```

若要檢查所有信任網域中 (DFS) 參照分散式檔案系統，請輸入：

```
dfsdiag /testreferral /DFSpath:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [dfsdiag 命令](dfsdiag.md)

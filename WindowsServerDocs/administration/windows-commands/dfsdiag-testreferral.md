---
title: dfsdiag testreferral
description: Dfsdiag testreferral 命令的參考文章，它會檢查分散式檔案系統 (DFS) 的參照。
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21ed7a6dd56fda0a6185f3f5aaa2a15d9d6fb565
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891123"
---
# <a name="dfsdiag-testreferral"></a>dfsdiag testreferral

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由執行下列測試，檢查分散式檔案系統 (DFS) 的參照：

- 如果您使用不含引數的**DFSpath*** 參數，此命令會驗證參考清單是否包含所有受信任的網域。

- 如果您指定網域，此命令會執行網域控制站的健康狀態檢查 (`dfsdiag /testdcs`) 並測試本機主機的網站關聯和網域快取。

- 如果您指定網域和 \SYSvol 或 \NETLOGON，此命令會執行相同的網域控制站健康情況檢查，並檢查 SYSvol 的存留時間** (TTL) **或 NETLOGON 參考是否符合900秒的預設值。

- 如果您指定命名空間根目錄，此命令會執行相同的網域控制站健康情況檢查，以及執行 DFS 設定檢查 (`dfsdiag /testdfsconfig`) ，並 () 的命名空間完整性檢查 `dfsdiag /testdfsintegrity` 。

- 如果您指定 (連結) 的 DFS 資料夾，此命令會執行相同的命名空間根健康狀態檢查，以及驗證資料夾目標的網站設定 (dfsdiag/testsites) 以及驗證本機主機的網站關聯。

## <a name="syntax"></a>語法

```
dfsdiag /testreferral /DFSpath:<DFS path to get referrals> [/full]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /DFSpath:`<path to get referrals>` | 可以是下列其中一項：<ul><li>**空白：** 僅測試受信任的網域。</li><li>`\\Domain:`只測試網域控制站的參考。</li><li>`\\Domain\SYSvol:`只測試 SYSvol 的參考。</li><li>`\\Domain\NETLOGON:`僅測試 NETLOGON 參考。</li><li>`\\<domain or server>\<namespace root>:`僅測試命名空間的根參考。</li><li>`\\<domain or server>\<namespace root>\<DFS folder>:`只測試 DFS 資料夾 (連結) 參考。</li></ul> |
| /full | 僅適用于網域和根參照。 確認登錄與 active directory 網域服務 (AD DS) 之間的網站關聯資訊是否一致。 |

## <a name="examples"></a>範例

若要在*com\MyNamespace*中檢查分散式檔案系統 (DFS) 的參照，請輸入：

```
dfsdiag /testreferral /DFSpath:\\contoso.com\MyNamespace
```

若要檢查所有受信任網域中的分散式檔案系統 (DFS) 的參照，請輸入：

```
dfsdiag /testreferral /DFSpath:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [dfsdiag 命令](dfsdiag.md)

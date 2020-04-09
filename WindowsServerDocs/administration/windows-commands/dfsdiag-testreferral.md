---
title: dfsdiag TestReferral
description: 適用于 dfsdiag TestReferral 的 Windows 命令主題，它會檢查分散式檔案系統（DFS）參照。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5c0a75d557d816ac9e19a1e22b3273195b93f53
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846242"
---
# <a name="dfsdiag-testreferral"></a>dfsdiag TestReferral

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由執行下列測試來檢查分散式檔案系統（DFS）的參考：

- 當您使用不含引數的 DFSpath 參數時，此命令會驗證參考清單是否包含所有受信任的網域。

- 當您指定網域時，此命令會執行網域控制站（dfsdiag/testdcs）的健全狀況檢查，並測試本機主機的網站關聯和網域快取。

- 當您指定網域和 \SYSvol 或 \NETLOGON 時，除了執行與指定網域相同的健全狀況檢查之外，此命令會檢查 SYSvol 的存留時間（\TTL）是否符合900秒的預設值。

- 當您指定命名空間根時，除了執行與指定網域相同的健全狀況檢查之外，此命令會執行 DFS 設定檢查（dfsdiag/TestDFSConfig）和命名空間完整性檢查（dfsdiag/TestDFSIntegrity）。

- 當您指定 DFS 資料夾（連結）時，除了執行您指定命名空間根目錄時的相同健康情況檢查，此命令會驗證資料夾目標的網站設定（dfsdiag/testsites），並驗證本機主機的網站關聯。

## <a name="syntax"></a>語法

```
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]
```

#### <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
| /DFSpath：<path for getting referrals>|這個 DFS 路徑可以是下列其中一項：<p>-   \(空白\)：測試受信任的網域。<br />-   \\\\網域：網域控制站的參考。<br />-   \\\\網域\\SYSvol： SYSvol 的參照。<br />\\NETLOGON 中的 -   \\\\功能變數名稱 i： NETLOGON 參考。<br />-   \\\\<Domain or server>\\<Namespace Root>：命名空間的根參考。<br />-   \\\\<Domain or server>\\<Namespace root>\\<DFS folder>： DFS 資料夾 \(連結\) 的參考。|
|/Full|僅適用于網域和根參照。 確認登錄與 active directory 網域服務 \(AD DS\)之間的網站關聯資訊是否一致。|

## <a name="examples"></a><a name=BKMK_Examples></a>典型

```
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace
```

```
dfsdiag /TestReferral /DFSpath:
```

## <a name="additional-references"></a>其他參考資料

-   - [命令列語法關鍵](command-line-syntax-key.md)



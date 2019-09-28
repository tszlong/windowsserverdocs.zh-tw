---
title: dfsdiag TestReferral
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: af22520d2c89f9d9f9d91ea6f43a33f3ff9c57f1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378364"
---
# <a name="dfsdiag-testreferral"></a>dfsdiag TestReferral

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由執行下列測試，檢查分散式檔案系統 @no__t 0DFS @ no__t-1 的參考：  
  
-   當您使用不含引數的 DFSpath 參數時，此命令會驗證參考清單是否包含所有受信任的網域。  
  
-   當您指定網域時，此命令會執行網域控制站的健康狀態檢查 \(dfsdiag \/testdcs @ no__t-2，並測試本機主機的網站關聯和網域快取。  
  
-   當您指定網域和 @no__t 0SYSvol 或 @no__t 1NETLOGON 時，除了執行與指定網域相同的健全狀況檢查之外，此命令會檢查存留時間 \(TTL @ no__t-3 （SYSvol 或 NETLOGON 參考）是否符合的預設值900秒。  
  
-   當您指定命名空間根時，除了執行與指定網域相同的健全狀況檢查之外，此命令也會執行 DFS 設定檢查 \(dfsdiag \/TestDFSConfig @ no__t-2，以及命名空間完整性檢查 \(dfsdiag \/TestDFSIntegrity @ no__t-5。  
  
-   當您指定 \(link @ no__t-1 的 DFS 資料夾時，除了執行與您指定命名空間根目錄時相同的健康情況檢查，此命令會驗證資料夾目標的網站設定 \(dfsdiag \/testsites @ no__t-4 並進行驗證本機主機的網站關聯。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]  
```  
  
### <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|\/DFSpath： <path for getting referrals>|這個 DFS 路徑可以是下列其中一項：<br /><br />-    @ no__t-1blank @ no__t-2：測試信任的網域。<br />-    @ no__t-1 @ no__t-2Domain：網域控制站的參照。<br />-    @ no__t-1 @ no__t-2Domain @ no__t-3SYSvol：SYSvol 的參考。<br />-    @ no__t-1 @ no__t-2Domain @ no__t-3NETLOGON：NETLOGON 參考。<br />-    @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5：命名空間的根參考。<br />-    @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7：DFS 資料夾 \(link @ no__t-1 個參考。|  
|\/Full|僅適用于網域和根參照。 確認登錄與 active directory 網域服務 @no__t 0AD DS @ no__t-1 之間的網站關聯資訊是否一致。|  
  
## <a name="BKMK_Examples"></a>典型  
若要 TBD，請輸入：  
  
```  
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace  
```  
  
若要 TBD，請輸入：  
  
```  
dfsdiag /TestReferral /DFSpath:  
```  
  
## <a name="additional-references"></a>其他參考  
  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  


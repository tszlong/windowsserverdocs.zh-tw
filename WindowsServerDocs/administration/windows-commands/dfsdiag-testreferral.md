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

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由執行下列測試，檢查分散式檔案系統 \(DFS\) 的參照：  
  
-   當您使用不含引數的 DFSpath 參數時，此命令會驗證參考清單是否包含所有受信任的網域。  
  
-   當您指定網域時，此命令會執行網域控制站的健康狀態檢查 \(dfsdiag \/testdcs\) 並測試本機主機的網站關聯和網域快取。  
  
-   當您指定網域並 \\SYSvol 或 \\NETLOGON 時，除了執行與指定網域相同的健康情況檢查之外，此命令會檢查 SYSvol 的存留時間 \(TTL\) 或 NETLOGON 參考是否符合900秒的預設值。  
  
-   當您指定命名空間根時，除了執行與指定網域相同的健全狀況檢查之外，此命令會執行 DFS 設定檢查 \(dfsdiag \/TestDFSConfig\)，以及命名空間完整性檢查 \(dfsdiag \/TestDFSIntegrity\)。  
  
-   當您指定 \(連結\)的 DFS 資料夾時，除了執行與指定命名空間根目錄相同的健全狀況檢查之外，此命令會驗證資料夾目標的網站設定 \(dfsdiag \/testsites\) 並驗證本機主機的網站關聯。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]  
```  
  
### <a name="parameters"></a>Parameters  
  
|參數|描述|  
|-------|--------|  
|\/DFSpath：<path for getting referrals>|這個 DFS 路徑可以是下列其中一項：<br /><br />-   \(空白\)：測試受信任的網域。<br />-   \\\\網域：網域控制站的參考。<br />-   \\\\網域\\SYSvol： SYSvol 的參照。<br />-   \\\\網域\\NETLOGON： NETLOGON 參考。<br />-   \\\\<Domain or server>\\<Namespace Root>：命名空間的根參考。<br />-   \\\\<Domain or server>\\<Namespace root>\\<DFS folder>： DFS 資料夾 \(連結\) 的參考。|  
|\/完整|僅適用于網域和根參照。 確認登錄與 active directory 網域服務 \(AD DS\)之間的網站關聯資訊是否一致。|  
  
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
  


---
title: Dfsdiag TestReferral
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cd1b87befa8a9cfda5ea27a4ce5a5105ea1a1009
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848019"
---
# <a name="dfsdiag-testreferral"></a>Dfsdiag TestReferral

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

檢查 分散式檔案系統\(DFS\)轉介藉由執行下列測試：  
  
-   當您使用不含引數的 DFSpath 參數時，此命令會驗證轉介清單包含所有信任的網域。  
  
-   當您指定網域時，此命令會執行網域控制站的健全狀況檢查\(dfsdiag \/testdcs\)並測試的站台關聯，以及本機主機的網域快取。  
  
-   當您指定網域並\\SYSvol 或\\NETLOGON，除了執行相同的健全狀況檢查當您指定網域，該命令會檢查的存留時間\(TTL\) SYSvol 或 NETLOGON 轉介的符合預設值為 900 秒。  
  
-   當您指定命名空間根目錄，以及執行相同的健康情況檢查，當您指定網域，此命令會執行 DFS 組態檢查\(dfsdiag \/TestDFSConfig\)和命名空間的完整性檢查\(dfsdiag \/TestDFSIntegrity\)。  
  
-   當您指定的 DFS 資料夾\(連結\)，以及執行相同的健康情況檢查，當您指定命名空間根目錄，此命令會驗證資料夾目標的站台組態\(dfsdiag \/testsites\)和驗證站台關聯的本機主機。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]  
```  
  
### <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|\/DFSpath:<path for getting referrals>|此 DFS 路徑可以是下列其中一項：<br /><br />-   \(空白\):測試受信任的網域。<br />-   \\\\網域：網域控制站轉介。<br />-   \\\\網域\\SYSvol:SYSvol 轉介。<br />-   \\\\網域\\NETLOGON:NETLOGON 轉介。<br />-   \\\\<Domain or server>\\<Namespace Root>:命名空間根目錄轉介。<br />-   \\\\<Domain or server>\\<Namespace root>\\<DFS folder>:DFS 資料夾\(連結\)轉介。|  
|\/完整|只能套用至網域和根的轉介。 確認站台登錄和 active directory 網域服務之間的關聯資訊的一致性\(AD DS\)。|  
  
## <a name="BKMK_Examples"></a>範例  
若待決定，要輸入：  
  
```  
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace  
```  
  
若待決定，要輸入：  
  
```  
dfsdiag /TestReferral /DFSpath:  
```  
  
## <a name="additional-references"></a>其他參考資料  
  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  


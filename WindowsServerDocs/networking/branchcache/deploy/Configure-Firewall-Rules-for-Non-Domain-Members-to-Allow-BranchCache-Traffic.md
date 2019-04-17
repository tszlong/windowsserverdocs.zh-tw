---
title: 設定適用於非網域成員允許 BranchCache 流量免
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da956be0-c92d-46ea-99eb-85e2bd67bf07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e5f744141efc35bb493bcd95fad53eafbbc3f78d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-firewall-rules-for-non-domain-members-to-allow-branchcache-traffic"></a>設定適用於非網域成員允許 BranchCache 流量免

>適用於：Windows Server（以每年次管道）、Windows Server 2016

設定第三方防火牆你並手動設定，允許快取分散式的模式執行 BranchCache 免 client 電腦，您可以使用此主題中的資訊。  
  
> [!NOTE]  
> -   如果您已經設定 BranchCache client 電腦使用群組原則、 群組原則設定覆寫任何手動 client 電腦套用原則設定。  
> -   如果您有部署 BranchCache DirectAccess 使用，您可以使用此主題中設定設定允許 BranchCache 流量 IPsec 規則。  
  
資格在**系統管理員**，或相當於的最低需求變更這些設定。  
  
## <a name="ms-pccrd-peer-content-caching-and-retrieval-discovery-protocol"></a>[MS PCCRD]: 對等內容快取，並擷取探索通訊協定  
快取分散式的戶端必須允許輸入 / 輸出 MS-PCCRD 流量的 Web 服務動態探索 （WS 探索） 通訊協定中執行。  
  
防火牆設定必須允許多點的流量輸入 / 輸出流量除了。 您可以使用下列設定來設定防火牆例外分散式快取模式。  
  
多點 IPv4 傳送： 239.255.255.250  
  
多點 IPv6 傳送： FF02::C  
  
輸入流量： 本機連接埠： 3702，遠端連接埠： 暫時  
  
輸出流量： 本機連接埠： 暫時、 遠端連接埠： 3702  
  
計畫： %systemroot%\system32\svchost.exe （BranchCache 服務 [PeerDistSvc]）  
  
## <a name="ms-pccrr-peer-content-caching-and-retrieval-retrieval-protocol"></a>[MS PCCRR]: 對等內容快取，並擷取： 擷取通訊協定  
快取分散式的戶端必須允許輸入 / 輸出 MS-PCCRR 流量的 HTTP 1.1 通訊協定執行如要求意見 (RFC) 2616年中所述。  
  
防火牆設定必須允許輸入 / 輸出流量。 您可以使用下列設定來設定防火牆例外分散式快取模式。  
  
輸入流量： 本機連接埠： 80、 遠端連接埠： 暫時  
  
輸出流量： 本機連接埠： 暫時、 遠端連接埠： 80  
  



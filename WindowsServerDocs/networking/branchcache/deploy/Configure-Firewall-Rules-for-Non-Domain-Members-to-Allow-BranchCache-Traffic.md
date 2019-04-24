---
title: 將非網域成員的防火牆規則設定為允許 BranchCache 流量
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da956be0-c92d-46ea-99eb-85e2bd67bf07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 288865f0237969e0bed7e105f8d539759984275e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834769"
---
# <a name="configure-firewall-rules-for-non-domain-members-to-allow-branchcache-traffic"></a>將非網域成員的防火牆規則設定為允許 BranchCache 流量

>適用於：Windows Server （半年通道），Windows Server 2016

若要設定協力廠商防火牆產品，並手動設定防火牆規則以允許 BranchCache 分散式快取模式中執行的用戶端電腦，您可以使用本主題中的資訊。  
  
> [!NOTE]  
> -   如果您已設定 BranchCache 用戶端電腦使用群組原則，群組原則設定覆寫任何手動設定来套用原則的用戶端電腦。  
> -   如果您已部署 BranchCache，有了 DirectAccess 之後，您可以使用本主題中的設定來設定以允許 BranchCache 流量的 IPsec 規則。  
  
中的成員資格**系統管理員**，或同等權限才能進行這些設定變更最小值。  
  
## <a name="ms-pccrd-peer-content-caching-and-retrieval-discovery-protocol"></a>[MS-PCCRD]:對等內容的快取和抓取探索通訊協定  
分散式快取的用戶端必須允許輸入和輸出 MS PCCRD 流量，這執行中的 Web 服務動態探索 (Ws-discovery) 通訊協定。  
  
防火牆設定必須允許多點傳送的流量，除了輸入和輸出流量。 您可以使用下列設定來設定防火牆例外以執行分散式快取模式。  
  
IPv4 多點傳送：239.255.255.250  
  
IPv6 多點傳送：FF02::C  
  
輸入流量：本機連接埠：3702，遠端連接埠： 暫時  
  
輸出流量：本機連接埠： 暫時的遠端連接埠：3702  
  
程式： %systemroot%\system32\svchost.exe （BranchCache 服務 [PeerDistSvc]）  
  
## <a name="ms-pccrr-peer-content-caching-and-retrieval-retrieval-protocol"></a>[MS-PCCRR]:對等內容的快取和擷取：擷取通訊協定  
分散式快取的用戶端必須允許輸入和輸出 MS PCCRR 流量，如要求建議 (RFC) 2616年中所述，在 HTTP 1.1 通訊協定中執行。  
  
防火牆設定必須允許輸入和輸出流量。 您可以使用下列設定來設定防火牆例外以執行分散式快取模式。  
  
輸入流量：本機連接埠：80，遠端連接埠： 暫時  
  
輸出流量：本機連接埠： 暫時的遠端連接埠：80  
  



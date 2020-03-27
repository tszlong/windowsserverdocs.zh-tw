---
title: 將非網域成員的防火牆規則設定為允許 BranchCache 流量
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da956be0-c92d-46ea-99eb-85e2bd67bf07
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 4a86d37fe8744127a91b7fb89e4f34d4a0a021fa
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318489"
---
# <a name="configure-firewall-rules-for-non-domain-members-to-allow-branchcache-traffic"></a>將非網域成員的防火牆規則設定為允許 BranchCache 流量

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題中的資訊來設定協力廠商防火牆產品，以及手動設定具有防火牆規則的用戶端電腦，讓 BranchCache 以分散式快取模式執行。  
  
> [!NOTE]  
> -   如果您已使用群組原則設定 BranchCache 用戶端電腦，則群組原則設定會覆寫套用原則之用戶端電腦的任何手動設定。  
> -   如果您已使用 DirectAccess 部署 BranchCache，您可以使用本主題中的設定來設定 IPsec 規則，以允許 BranchCache 流量。  
  
若要進行這些設定變更，至少需要**Administrators**的成員資格或同等許可權。  
  
## <a name="ms-pccrd-peer-content-caching-and-retrieval-discovery-protocol"></a>[MS-PCCRD]：對等內容快取和抓取探索通訊協定  
分散式快取用戶端必須允許輸入和輸出 MS PCCRD 流量，這會在 Web 服務動態探索（WS-Discovery）通訊協定中攜帶。  
  
除了輸入和輸出流量外，防火牆設定也必須允許多播流量。 您可以使用下列設定來設定分散式快取模式的防火牆例外。  
  
IPv4 多播：239.255.255.250  
  
IPv6 多播： FF02：： C  
  
輸入流量：本機埠：3702、遠端埠：暫時  
  
輸出流量：本機埠：暫時、遠端埠：3702  
  
程式：%systemroot%\system32\svchost.exe （BranchCache 服務 [PeerDistSvc]）  
  
## <a name="ms-pccrr-peer-content-caching-and-retrieval-retrieval-protocol"></a>[MS-PCCRR]：對等內容快取和抓取：抓取通訊協定  
分散式快取用戶端必須允許傳入和傳出的 PCCRR 流量，如要求建議（RFC）2616中所述，會以 HTTP 1.1 通訊協定傳送。  
  
防火牆設定必須允許輸入和輸出流量。 您可以使用下列設定來設定分散式快取模式的防火牆例外。  
  
輸入流量：本機埠：80、遠端埠：暫時  
  
輸出流量：本機埠：暫時、遠端埠：80  
  



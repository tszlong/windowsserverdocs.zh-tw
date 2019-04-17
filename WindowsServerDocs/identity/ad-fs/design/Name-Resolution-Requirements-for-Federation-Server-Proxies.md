---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: "聯盟的 Proxy 伺服器的名稱解析需求"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a94e4de181cd8794d479bbd6695a94658aba0f86
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>聯盟的 Proxy 伺服器的名稱解析需求

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

時 client 網際網路上的電腦嘗試存取 Active Directory 同盟服務 \(AD FS\) 受保護的應用程式，就必須先驗證聯盟伺服器。 在大部分案例中，聯盟伺服器通常不是從網際網路直接存取。 因此，client 網際網路的電腦必須重新導向至聯盟 proxy 伺服器改為。 您可以完成成功重新導向適當的網域名稱系統 \(DNS\) 記錄新增到您的 DNS 區域或面臨網際網路的區域。  
  
您將網際網路戶端聯盟 proxy 伺服器使用方法您如何設定 DNS 區域周邊網路，或您在網際網路上設定，您可以控制 DNS 區域的方式而定。 聯盟伺服器 proxy 是針對使用周邊網路。 他們將網際網路 client 要求聯盟伺服器成功僅時 DNS 已正確設定所有的 Internet\ 面向區域中，您可以控制。 因此，您的 Internet\ 面向區域的設定，您有 DNS 波周邊網路或 DNS 區域波周邊網路和網際網路戶端-很重要。  
  
本主題描述您可能需要設定時聯盟 proxy 伺服器置於周邊網路的名稱解析步驟。 若要判斷要遵循的步驟，第一次判斷其中一項下列的 DNS 案例最符合您的組織的周邊網路的 DNS 基礎結構。 然後，依照該案例。  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>波周邊網路的 DNS 區域  
在本案例中，您的組織有一或兩個 DNS 區域中周邊網路和網際網路上的任何 DNS 區域無法控制您的組織。 聯盟伺服器 proxy 做只周邊網路案例 DNS 區域中的成功名稱解析而定下列條件：  
  
-   聯盟 proxy 伺服器設定必須在主機檔案解析完整的網域名稱，\(FQDN\) 的聯盟伺服器端點 URL 聯盟伺服器叢集聯盟伺服器的 IP 位址。  
  
-   Account 協力廠商周邊網路的 DNS 必須使聯盟伺服器端點 URL 的 FQDN 解析為聯盟 proxy 伺服器的 IP 位址設定。  
  
下圖和對應步驟顯示如何針對特定範例實現每個條件。 在此範例中，Microsoft 網路負載平衡 \(NLB\) 技術使用現有的聯盟伺服器陣列提供單一、叢集 FQDN 和單一、叢集 IP 位址。  
  
![名稱需求](media/adfs2_deploy_single_fs.gif)  
  
查看更多有關如何設定叢集 IP 位址或叢集 FQDN 使用 NLB，[指定叢集參數](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1.主機上的檔案聯盟 proxy 伺服器設定  
Account 合作夥伴聯盟伺服器 proxy 解析所有要求 fs.fabrikam.com account 聯盟伺服器 proxy 設定 DNS 周邊網路中的，因為有解析實際 account 聯盟伺服器的 IP 位址 fs.fabrikam.com 其主機本機檔案中的項目 \（或聯盟伺服器 farm\ 叢集 DNS 名稱），已連接到企業網路。 這樣可以 account 聯盟伺服器 proxy 解析主機名稱 fs.fabrikam.com account 聯盟伺服器，而對其本身 — 如果您嘗試使用周邊 DNS fs.fabrikam.com 上看起來就會發生在以便聯盟 proxy 伺服器可以與聯盟伺服器通訊。  
  
### <a name="2-configure-perimeter-dns"></a>2.周邊 DNS 設定  
因為只能在單一 AD FS 主機名稱 client 電腦的指示來-無論是在企業網路或網際網路上 — client 在網際網路上的電腦使用周邊 DNS 伺服器必須解析 account 聯盟伺服器 proxy 周邊網路上的 IP 位址 account 聯盟伺服器 \(fs.fabrikam.com\) 的 FQDN。 它可以嘗試解析 fs.fabrikam.com 時，向前戶端入 account 聯盟伺服器 proxy，周邊 DNS 包含有限的 corp.fabrikam.com DNS 區域 fs \(fs.fabrikam.com\) 的單一主機 \(A\) 資源記錄與 account 聯盟伺服器 proxy 周邊網路上的 IP 位址。  
  
如需有關如何修改主機聯盟 proxy 伺服器的檔案及設定 DNS 周邊網路中的資訊，請查看[設定為聯盟伺服器 Proxy 做周邊網路 DNS 區域中的名稱解析](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md)。  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>波周邊網路和網際網路戶端 DNS 區域  
在本案例中，您的組織控制 DNS 區域周邊網路和網際網路上的至少一個 DNS 區域。 本案例中聯盟伺服器 proxy 成功的名稱解析而定下列條件：  
  
-   DNS account 協力廠商的網際網路區域必須使 FQDN 聯盟伺服器主機名稱解析為 IP 位址聯盟伺服器 proxy 周邊網路中的設定。  
  
-   Account 協力廠商周邊網路的 DNS 必須使 FQDN 聯盟伺服器主機名稱解析為聯盟公司網路中伺服器的 IP 位址設定。  
  
下圖和對應步驟顯示如何針對特定範例實現每個條件。  
  
![名稱需求](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1.周邊 DNS 設定  
本案例中，因為它假設您將會設定網際網路 DNS 區域您控制解析要求的針對特定端點 URL \ (也就是 fs.fabrikam.com\) 聯盟伺服器周邊網路 proxy，您還必須設定區域 DNS 伺服器聯盟公司網路中的這些要求轉送給周邊設備中。  
  
這樣可以戶端轉送 account 聯盟伺服器嘗試解析 fs.fabrikam.com 時，周邊 DNS fs \(fs.fabrikam.com\) 和伺服器的 IP 位址 account 聯盟公司網路上的單一主機 \(A\) 資源記錄設定。 這樣可以 account 聯盟伺服器 proxy 解析主機名稱 fs.fabrikam.com account 聯盟伺服器，而對其本身 — 如果您嘗試使用網際網路 DNS fs.fabrikam.com 上看起來就會發生在以便聯盟 proxy 伺服器可以與聯盟伺服器通訊。  
  
### <a name="2-configure-internet-dns"></a>2.設定網際網路 DNS  
名稱解析為成功在本案例中，針對所有要求 fs.fabrikam.com 從網際網路上的 client 電腦必須都解析網際網路 DNS 區域，您可以控制。 因此，您必須設定您的網際網路 DNS 區域轉送 client 要求 fs.fabrikam.com account 聯盟伺服器 proxy 周邊網路的 IP 位址。  
  
如需如何修改周邊網路和網際網路 DNS 區域的相關資訊，請查看[的 DNS 區域，提供同時周邊網路和網際網路戶端聯盟伺服器 Proxy 設定名稱解析](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md)。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

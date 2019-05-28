---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: 同盟伺服器 Proxy 的名稱解析需求
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8aef8b3d8f1e6dde4f960a3bee5a93964d07c72b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191280"
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>同盟伺服器 Proxy 的名稱解析需求

當網際網路上的用戶端電腦嘗試存取由 Active Directory Federation Services 保護的應用程式\(AD FS\)，他們必須先進行驗證的同盟伺服器。 在大部分情況下，同盟伺服器通常不是直接從網際網路存取。 因此，網際網路用戶端電腦必須重新導向至同盟伺服器 proxy 改為。 您可以藉由新增適當的網域名稱系統來完成成功的重新導向\(DNS\)記錄到您的 DNS 區域或面對網際網路的區域。  
  
您用來將網際網路用戶端重新導向至同盟伺服器 proxy 的方法取決於您在周邊網路中設定 DNS 區域的方式，或您在網際網路上設定 DNS 區域，您可以控制的方式。 同盟伺服器 proxy 是用於周邊網路。 它們網際網路用戶端將要求重新導向至同盟伺服器成功 DNS 已正確設定中的所有網際網路時，才\-面向控制的區域。 因此，您的網際網路設定\-面向的區域，您是否有提供服務僅對周邊網路的 DNS 區域或周邊網路和網際網路用戶端提供服務的 DNS 區域 — 很重要。  
  
本主題描述設定名稱解析，當您將同盟伺服器 proxy 在周邊網路時，可採取的步驟。 若要判斷應遵循哪些步驟，請先判斷下列哪一個 DNS 案例最符合您組織的周邊網路中的 DNS 基礎結構。 然後，遵循該案例中的步驟。  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>僅對周邊網路提供服務的 DNS 區域  
在此案例中，您的組織在周邊網路中有一或兩個 DNS 區域，且您的組織不會控制在網際網路上的任何 DNS 區域。 在 DNS 區域，可僅周邊網路案例的同盟伺服器 proxy 的成功名稱解析取決於下列條件：  
  
-   同盟伺服器 proxy 設定必須在主機檔案中的完整的網域名稱解析\(FQDN\)的同盟伺服器或同盟伺服器叢集的 IP 位址的同盟伺服器端點 URL。  
  
-   必須設定帳戶夥伴的周邊網路中的 DNS，以便同盟伺服器端點 URL 的 FQDN 解析為同盟伺服器 proxy 的 IP 位址。  
  
下圖和相對應的步驟顯示如何針對指定的範例達成每一個條件。 在此圖中，Microsoft 網路負載平衡\(NLB\)技術提供單一、 叢集 FQDN 和一個現有的同盟伺服器陣列的叢集 IP 位址。  
  
![名稱需求](media/adfs2_deploy_single_fs.gif)  
  
如需有關設定叢集 IP 位址或叢集 FQDN 使用 NLB，請參閱[指定叢集參數](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1.在同盟伺服器 Proxy 上設定主機檔案。  
帳戶夥伴同盟伺服器 proxy 在周邊網路中的 DNS 設定解析帳戶同盟伺服器 proxy fs.fabrikam.com 的所有要求，因為有一個項目，在其本機主機檔案，以將 fs.fabrikam.com 的 IP 位址實際的帳戶同盟伺服器\(同盟伺服器陣列的叢集 DNS 名稱或\)，連線到公司網路。 這可讓帳戶同盟伺服器 proxy，來解析主機名稱 fs.fabrikam.com 解析帳戶同盟伺服器，而不是本身 — 如果它嘗試使用周邊 DNS 查詢 fs.fabrikam.com 就會發生如 — 以便同盟伺服器 proxy 可以與同盟伺服器通訊。  
  
### <a name="2-configure-perimeter-dns"></a>2.設定周邊 DNS  
因為只有單一 AD FS 主機名稱，用戶端電腦會被導向至 — 無論是在內部網路或網際網路上 — 在網際網路上使用周邊 DNS 伺服器的用戶端電腦必須解決的帳戶同盟伺服器的FQDN\(fs.fabrikam.com\)帳戶同盟伺服器 proxy 在周邊網路上的 IP 位址。 周邊網路 DNS，以便在嘗試解決 fs.fabrikam.com 時，它可以轉送至帳戶同盟伺服器 proxy 的用戶端，包含限制的 corp.fabrikam.com DNS 區域的單一主機\(A\) fs的資源記錄\(fs.fabrikam.com\)和帳戶同盟伺服器 proxy 在周邊網路上的 IP 位址。  
  
如需如何修改主機檔案的同盟伺服器 proxy，並在周邊網路中設定 DNS 的詳細資訊，請參閱[設定 DNS 區域，可僅對周邊網路中的同盟伺服器Proxy的名稱解析](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md).  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>對周邊網路和網際網路用戶端提供服務的 DNS 區域  
在此案例中，您的組織控制周邊網路中的 DNS 區域，和至少一個網際網路上的 DNS 區域。 在此案例中的同盟伺服器 proxy 的成功名稱解析是根據下列條件而定：  
  
-   必須設定帳戶夥伴在網際網路區域中的 DNS，以便同盟伺服器主機名稱的 FQDN 解析為周邊網路中的同盟伺服器 proxy 的 IP 位址。  
  
-   必須設定帳戶夥伴的周邊網路中的 DNS，以便同盟伺服器主機名稱的 FQDN 解析為公司網路中的同盟伺服器的 IP 位址。  
  
下圖和相對應的步驟顯示如何針對指定的範例達成每一個條件。  
  
![名稱需求](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1.設定周邊 DNS  
此案例中，因為它會假設您將設定的網際網路 DNS 區域，您可以控制来解決的要求特定的端點 url\(也就是 fs.fabrikam.com\)中同盟伺服器 proxy周邊網路中，您也必須設定該區域中的周邊 DNS，將這些要求轉送到公司網路中的同盟伺服器。  
  
周邊網路 DNS 以便用戶端可以轉送至帳戶同盟伺服器，嘗試解決 fs.fabrikam.com 時，已使用單一主機\(A\) fs 資源記錄\(fs.fabrikam.com\)與公司網路上的帳戶同盟伺服器的 IP 位址。 這可讓帳戶同盟伺服器 proxy 帳戶同盟伺服器，而不是本身解析主機名稱 fs.fabrikam.com 解析 — 如果它嘗試使用網際網路 DNS 查詢 fs.fabrikam.com 就會發生如 — 讓同盟伺服器proxy 可以與同盟伺服器通訊。  
  
### <a name="2-configure-internet-dns"></a>2.設定網際網路 DNS  
若要讓此案例的名稱解析成功，來自網際網路上用戶端電腦至 fs.fabrikam.com 的所有要求，必須透過您控制的網際網路 DNS 區域進行解析。 因此，您必須設定您的網際網路 DNS 區域轉送至帳戶同盟伺服器 proxy 在周邊網路的 IP 位址的 fs.fabrikam.com 的用戶端要求。  
  
如需如何修改周邊網路和網際網路 DNS 區域的詳細資訊，請參閱[設定中的 DNS 區域，可同時在周邊網路和網際網路用戶端的同盟伺服器 Proxy 的名稱解析](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

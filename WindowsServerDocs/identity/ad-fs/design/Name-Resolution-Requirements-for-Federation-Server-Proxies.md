---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: 同盟伺服器 Proxy 的名稱解析需求
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 540b58c6fe150e5d8781b1d23e97a3efcdf6c43d
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959800"
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>同盟伺服器 Proxy 的名稱解析需求

當網際網路上的用戶端電腦嘗試存取受 Active Directory 同盟服務 AD FS 保護的應用程式 \( 時 \) ，必須先向同盟伺服器進行驗證。 在大部分情況下，同盟伺服器通常無法直接從網際網路存取。 因此，網際網路用戶端電腦必須改為重新導向至同盟伺服器 proxy。 您可以藉由將適當的網域名稱系統 \( DNS \) 記錄新增至您的 DNS 區域或面對網際網路的區域，來完成成功的重新導向。  
  
您用來將網際網路用戶端重新導向至同盟伺服器 proxy 的方法，取決於您在周邊網路中設定 DNS 區域的方式，或如何設定您在網際網路上控制的 DNS 區域。 同盟伺服器 proxy 是用於周邊網路。 只有當 DNS 已在您控制的所有網際網路對向區域中正確設定時，它們才會成功地將網際網路用戶端要求重新導向至同盟伺服器 \- 。 因此， \- 如果您的 dns 區域僅提供周邊網路或服務周邊網路和網際網路用戶端的 dns 區域，則設定網際網路對向區域很重要。  
  
本主題說明當您在周邊網路中放置同盟伺服器 proxy 時，可以採取的步驟來設定名稱解析。 若要判斷應遵循哪些步驟，請先判斷下列哪一個 DNS 案例最符合您組織的周邊網路中的 DNS 基礎結構。 然後，遵循該案例中的步驟。  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>僅對周邊網路提供服務的 DNS 區域  
在此案例中，您的組織在周邊網路中有一或兩個 DNS 區域，且您的組織不會控制在網際網路上的任何 DNS 區域。 僅服務周邊網路案例之 DNS 區域中的同盟伺服器 proxy 的成功名稱解析，取決於下列條件：  
  
-   同盟伺服器 proxy 必須要有 hosts 檔案中的設定，才能將 \( 同盟伺服器端點 URL 的完整功能變數名稱 FQDN 解析 \) 為同盟伺服器或同盟伺服器叢集的 IP 位址。  
  
-   必須設定帳戶夥伴的周邊網路中的 DNS，才能將同盟伺服器端點 URL 的 FQDN 解析為同盟伺服器 proxy 的 IP 位址。  
  
下圖和相對應的步驟顯示如何針對指定的範例達成每一個條件。 在此圖中，Microsoft 網路負載平衡 \( NLB \) 技術會為現有的同盟伺服器陣列提供單一、叢集 FQDN 和單一叢集 IP 位址。  
  
![名稱需求](media/adfs2_deploy_single_fs.gif)  
  
如需使用 NLB 設定叢集 IP 位址或叢集 FQDN 的詳細資訊，請參閱[指定叢集參數](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1.在同盟伺服器 Proxy 上設定主機檔案。  
因為周邊網路中的 DNS 設定為將 fs.fabrikam.com 的所有要求解析為帳戶同盟伺服器 proxy，所以帳戶夥伴同盟伺服器 proxy 在其本機主機檔案中具有專案，可將 fs.fabrikam.com 解析為 \( \) 連線到公司網路之同盟伺服器陣列的實際帳戶同盟伺服器或叢集 DNS 名稱的 IP 位址。 如此一來，帳戶同盟伺服器 proxy 就可以將主機名稱 fs.fabrikam.com 解析為帳戶同盟伺服器，而不是解析為本身—如果它嘗試使用周邊 DNS 來查詢 fs.fabrikam.com 就會發生這種情況，讓同盟伺服器 proxy 可以與同盟伺服器通訊。  
  
### <a name="2-configure-perimeter-dns"></a>2.設定周邊 DNS  
因為用戶端電腦只會被導向至單一 AD FS 主機名稱（無論是在內部網路或網際網路上），所以使用周邊 DNS 伺服器的網際網路上的用戶端電腦必須將帳戶同盟伺服器 fs.fabrikam.com 的 FQDN 解析 \( \) 為周邊網路上帳戶同盟伺服器 PROXY 的 IP 位址。 如此一來，當用戶端嘗試解析 fs.fabrikam.com 時，就可以將它們轉送到帳戶同盟伺服器 proxy，而周邊 DNS 則包含一個受限的 corp.fabrikam.com DNS 區域，其中具有一個 \( \) 適用于 fs fs.fabrikam.com 的資源記錄 \( \) ，以及周邊網路上帳戶同盟伺服器 proxy 的 IP 位址。  
  
如需有關如何修改同盟伺服器 proxy 的 hosts 檔案，以及在周邊網路中設定 DNS 的詳細資訊，請參閱[在僅服務周邊網路的 DNS 區域中設定同盟伺服器 proxy 的名稱解析](../deployment/configure-name-resolution-for-federation-server-proxy-in-dns-zone-serving-only-perimeter-network.md)。  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>對周邊網路和網際網路用戶端提供服務的 DNS 區域  
在此案例中，您的組織控制周邊網路中的 DNS 區域，和至少一個網際網路上的 DNS 區域。 在此案例中，同盟伺服器 proxy 的成功名稱解析取決於下列條件：  
  
-   必須設定帳戶夥伴的網際網路區域中的 DNS，讓同盟伺服器主機名稱的 FQDN 解析為周邊網路中同盟伺服器 proxy 的 IP 位址。  
  
-   必須設定帳戶夥伴的周邊網路中的 DNS，讓同盟伺服器主機名稱的 FQDN 解析為公司網路中同盟伺服器的 IP 位址。  
  
下圖和相對應的步驟顯示如何針對指定的範例達成每一個條件。  
  
![名稱需求](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1.設定周邊 DNS  
在此案例中，由於假設您將設定您控制的網際網路 DNS 區域，以解析針對特定端點 URL 所提出的要求，也就 \( 是 fs.fabrikam.com \) 到周邊網路中的同盟伺服器 proxy，您也必須在周邊 DNS 中設定該區域，以將這些要求轉送到公司網路中的同盟伺服器。  
  
如此一來，用戶端可以在嘗試解析 fs.fabrikam.com 時轉送到帳戶同盟伺服器，而周邊 DNS 則是以單一主機 \( 設定 \) fs fs.fabrikam.com 的資源 \( 記錄 \) ，以及公司網路上帳戶同盟伺服器的 IP 位址。 如此一來，帳戶同盟伺服器 proxy 就可以將主機名稱 fs.fabrikam.com 解析為帳戶同盟伺服器，而不是解析為本身—如果它嘗試使用網際網路 DNS 來查詢 fs.fabrikam.com，就會發生這種情況，讓同盟伺服器 proxy 可以與同盟伺服器通訊。  
  
### <a name="2-configure-internet-dns"></a>2.設定網際網路 DNS  
若要讓此案例的名稱解析成功，來自網際網路上用戶端電腦至 fs.fabrikam.com 的所有要求，必須透過您控制的網際網路 DNS 區域進行解析。 因此，您必須設定您的網際網路 DNS 區域，以將 fs.fabrikam.com 的用戶端要求轉送至周邊網路中帳戶同盟伺服器 proxy 的 IP 位址。  
  
如需有關如何修改周邊網路和網際網路 DNS 區域的詳細資訊，請參閱[在同時提供周邊網路和網際網路用戶端的 DNS 區域中設定同盟伺服器 Proxy 的名稱解析](../deployment/configure-name-resolution-for-federation-server-proxy-in-dns-zone-serving-only-perimeter-network.md)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

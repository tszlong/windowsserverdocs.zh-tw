---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: 僅適用於周邊網路及網際網路用戶端的 DNS 區域中設定同盟伺服器 Proxy 的名稱解析
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 118c03ada32d3cd5b198ecd238078984a38df0db
ms.sourcegitcommit: 8fbd2d877612a9feb02d7d91ed0372d7cd441d5c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71359826"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-both-the-perimeter-network-and-internet-clients"></a>僅適用於周邊網路及網際網路用戶端的 DNS 區域中設定同盟伺服器 Proxy 的名稱解析


因此，在 Active Directory 同盟服務的同盟伺服器 proxy 中，名稱解析可以順利運作 \(AD FS @ no__t-1 案例中，其中一或多個網域名稱系統 \(DNS @ no__t-3 區域同時提供周邊網路和網際網路用戶端，必須完成下列工作：  
  
-   您控制的網際網路區域中的 DNS 必須設定為將 AD FS 主機名稱的所有網際網路用戶端要求解析為同盟伺服器 proxy。 若要完成此動作，請將主機 \(A @ no__t-1 資源記錄新增至同盟伺服器 proxy 的網際網路 DNS 區域。  
  
-   周邊網路中的 DNS 必須設定為將 AD FS 主機名稱的所有連入用戶端要求，解析為同盟伺服器。 若要完成此動作，請將主機 \(A @ no__t-1 資源記錄新增至同盟伺服器 proxy 的周邊 DNS 區域。  
  
> [!NOTE]  
> 假設已在公司網路 DNS 中建立同盟伺服器的主機 \(A @ no__t-1 資源記錄。 如果此記錄尚未存在，請建立此記錄，然後執行這些程式。 如需有關如何為同盟伺服器建立主機 \(A @ no__t-1 資源記錄的詳細資訊，請參閱[將&#40;主機 a&#41;資源記錄新增至同盟伺服器的公司 DNS](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>將主機 \(A @ no__t-1 資源記錄新增至同盟伺服器 proxy 的網際網路 DNS 區域  
因此，網際網路上的用戶端電腦可以透過新部署的同盟伺服器 proxy 成功存取同盟伺服器，您必須先在您控制的網際網路 DNS 區域中建立主機 \(A @ no__t-1 資源記錄。 此資源記錄會將帳戶同盟伺服器的主機名稱 \(for 範例，fs. fabrikam .com @ no__t-1 解析為帳戶同盟伺服器 proxy 的 IP 位址，\(for 範例中為周邊網路中的 131.107.27.68 @ no__t-3。  
  
> [!NOTE]  
> 假設您使用執行 Windows 2000 Server、Windows Server 2003 或 Windows Server 2008 的 DNS 伺服器，並搭配 DNS 伺服器服務來控制網際網路 DNS 區域。  
  
若要完成此程式，至少需要**Administrators**的成員資格或同等許可權。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>若要將主機 \(A @ no__t-1 資源記錄新增至同盟伺服器 proxy 的網際網路 DNS 區域  
  
1.  在網際網路 DNS 區域的 DNS 伺服器上，開啟 DNS snap @ no__t-0in。  
  
2.  在主控台樹中，右 @ no__t-0click 適用的正向對應區域，然後按一下 [**新增主機] \(a 或 AAAA @ no__t-3**。  
  
3.  在 [**名稱**] 中，只輸入同盟伺服器的電腦名稱稱。 例如，針對完整功能變數名稱 \(FQDN @ no__t-1 fs.fabrikam.com，輸入**fs**。  
  
4.  在 [ **ip 位址**] 中，輸入新的同盟伺服器 PROXY 的 IP 位址，例如131.107.27.68。  
  
5.  按一下 [新增主機]。  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>將主機 \(A @ no__t-1 資源記錄新增至同盟伺服器 proxy 的周邊 DNS 區域  
為了讓同盟伺服器 proxy 可以成功處理網際網路用戶端要求，並在網際網路 DNS 區域解析後連線到同盟伺服器，您必須在周邊 DNS 區域中建立主機 \(A @ no__t-1 資源記錄。 此資源記錄會解析帳戶同盟伺服器的主機名稱 \(for 範例 fs。 fabrikam .com @ no__t-0 到帳戶同盟伺服器的 IP 位址 \(for 範例，在公司網路中 192.168.1.4 @ no__t-2。  
  
> [!NOTE]  
> 假設您使用的 DNS 伺服器執行 Windows 2000 Server、Windows Server 2003、Windows Server 2008 或 Windows Server®2012與 DNS 伺服器服務，以控制周邊 DNS 區域。  
  
若要完成此程式，至少需要**Administrators**的成員資格或同等許可權。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>若要將主機 \(A @ no__t-1 資源記錄新增至同盟伺服器 proxy 的周邊 DNS 區域  
  
1.  在周邊網路的 DNS 伺服器上，開啟**dns snap @ no__t-1in**。  
  
2.  在主控台樹中，右 @ no__t-0click 適用的正向對應區域，然後按一下 [**新增主機] \(a 或 AAAA @ no__t-3**。  
  
3.  在 [**名稱**] 中，只輸入同盟伺服器的電腦名稱稱。 例如，針對 FQDN fs.fabrikam.com，輸入 **fs**。  
  
4.  在 [ **ip 位址**] 文字方塊中，輸入公司網路中同盟伺服器的 IP 位址，例如192.168.1.4。  
  
5.  按一下 [新增主機]。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[同盟伺服器 Proxy 的名稱解析需求](https://technet.microsoft.com/library/dd807055.aspx)  
  


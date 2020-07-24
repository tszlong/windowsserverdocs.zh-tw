---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: 周邊和網際網路用戶端的名稱解析
author: billmath
manager: femila
ms.date: 04/13/2020
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 83257c34b5070f5be637d13aff07f4389fa1f9a1
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964760"
---
# <a name="name-resolution-for-perimeter-and-internet-clients"></a>周邊和網際網路用戶端的名稱解析


如此一來，就可以在 Active Directory 同盟服務 AD FS 案例中，將名稱解析成功地用於同盟伺服器 proxy \( \) ，其中一或多個網域名稱系統 \( DNS \) 區域同時提供周邊網路和網際網路用戶端，必須完成下列工作：  
  
-   您控制的網際網路區域中的 DNS 必須設定為將 AD FS 主機名稱的所有網際網路用戶端要求解析為同盟伺服器 proxy。 若要完成此動作，請將主機 \( a \) 資源記錄新增至同盟伺服器 Proxy 的網際網路 DNS 區域。  
  
-   周邊網路中的 DNS 必須設定為將 AD FS 主機名稱的所有連入用戶端要求，解析為同盟伺服器。 若要完成此動作，請將主機 \( a \) 資源記錄新增至同盟伺服器 proxy 的周邊 DNS 區域。  
  
> [!NOTE]  
> 假設已 \( \) 在公司網路 DNS 中建立同盟伺服器的主機 a 資源記錄。 如果此記錄尚未存在，請建立此記錄，然後執行這些程式。 如需如何為 \( 同盟伺服器建立資源記錄的主機的詳細資訊 \) ，請參閱[將主機 &#40;&#41; 資源記錄新增至同盟伺服器的公司 DNS](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>將主機 \( a \) 資源記錄新增至同盟伺服器 Proxy 的網際網路 DNS 區域  
因此，網際網路上的用戶端電腦可以透過新部署的同盟伺服器 proxy 成功存取同盟伺服器，您必須先 \( \) 在您控制的網際網路 DNS 區域中建立主機 a 資源記錄。 此資源記錄會解析帳戶同盟伺服器的主機名稱 \( ，例如，fs.fabrikam.com \) 至帳戶同盟伺服器 PROXY 的 IP 位址， \( 例如 \) 周邊網路中的131.107.27.68。  
  
> [!NOTE]  
> 假設您使用執行 Windows 2000 Server、Windows Server 2003 或 Windows Server 2008 的 DNS 伺服器，並搭配 DNS 伺服器服務來控制網際網路 DNS 區域。  
  
若要完成此程式，至少需要**Administrators**的成員資格或同等許可權。  請參閱在[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>將主機 \( a \) 資源記錄新增至同盟伺服器 Proxy 的網際網路 DNS 區域  
  
1.  在網際網路 DNS 區域的 DNS 伺服器上，開啟 [DNS] 嵌入式管理單元 \- 。  
  
2.  在主控台樹中，以滑鼠右鍵 \- 按一下適用的正向對應區域，然後按一下 [**新增主機 \( A] 或 [AAAA \) **]。  
  
3.  在 [**名稱**] 中，只輸入同盟伺服器的電腦名稱稱。 例如，針對完整功能變數名稱 \( FQDN \) fs.fabrikam.com，輸入**fs**。  
  
4.  在 [ **ip 位址**] 中，輸入新的同盟伺服器 PROXY 的 IP 位址，例如131.107.27.68。  
  
5.  按一下 [新增主機]****。  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>將主機 \( a \) 資源記錄新增至同盟伺服器 proxy 的周邊 DNS 區域  
為了讓同盟伺服器 proxy 可以成功處理網際網路用戶端要求，並在網際網路 DNS 區域解析後連線到同盟伺服器，您必須 \( \) 在周邊 DNS 區域中建立主機 a 資源記錄。 此資源記錄會解析帳戶同盟伺服器的主機名稱 \( ，例如 fs。 fabrikam.com \) 至帳戶同盟伺服器的 IP 位址 \( ，例如 \) 公司網路中的192.168.1.4。  
  
> [!NOTE]  
> 假設您使用的 DNS 伺服器執行 Windows 2000 Server、Windows Server 2003、Windows Server 2008 或 Windows Server &reg; 2012 與 Dns 伺服器服務，以控制周邊 dns 區域。  
  
若要完成此程式，至少需要**Administrators**的成員資格或同等許可權。  請參閱在[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>將主機 \( a \) 資源記錄新增至同盟伺服器 proxy 的周邊 DNS 區域  
  
1.  在周邊網路的 DNS 伺服器上，開啟 [Dns] 嵌入式**管理單元 \- **。  
  
2.  在主控台樹中，以滑鼠右鍵 \- 按一下適用的正向對應區域，然後按一下 [**新增主機 \( A] 或 [AAAA \) **]。  
  
3.  在 [**名稱**] 中，只輸入同盟伺服器的電腦名稱稱。 例如，對於 FQDN fs.fabrikam.com，請輸入 **fs**。  
  
4.  在 [ **ip 位址**] 文字方塊中，輸入公司網路中同盟伺服器的 IP 位址，例如192.168.1.4。  
  
5.  按一下 [新增主機]****。  
  
## <a name="additional-references"></a>其他參考  
[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[同盟伺服器 Proxy 的名稱解析需求](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807055(v=ws.11))  
  

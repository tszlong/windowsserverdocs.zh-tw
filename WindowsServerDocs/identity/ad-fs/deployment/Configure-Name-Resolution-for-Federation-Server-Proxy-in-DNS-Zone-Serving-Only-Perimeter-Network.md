---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: 在僅適用於周邊網路的 DNS 區域中設定同盟伺服器 Proxy 的名稱解析
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4c00fa371539109a3cb444ebd999f282dc70b771
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86953760"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>在僅適用於周邊網路的 DNS 區域中設定同盟伺服器 Proxy 的名稱解析


如此一來，在 \( \) 一或多個網域名稱系統 DNS 區域僅服務周邊網路的 Active Directory 同盟服務 AD FS 案例中，名稱解析才能成功地 \( \) 執行，必須完成下列工作：  
  
-   必須更新同盟伺服器 proxy 上的 hosts 檔案，才能新增同盟伺服器的 IP 位址。  
  
-   周邊網路中的 DNS 必須設定為將 AD FS 主機名稱的所有用戶端要求，解析為同盟伺服器 proxy。 若要這麼做，您可以將主機 \( a \) 資源記錄新增至同盟伺服器 proxy 的周邊 DNS。  
  
> [!NOTE]  
> 這些程式假設已 \( \) 在公司網路 DNS 中建立同盟伺服器的主機 a 資源記錄。 如果此記錄尚未存在，請建立此記錄，然後執行這些程式。 如需如何為同盟伺服器建立資源記錄之主機的詳細資訊 \( \) ，請參閱[將主機 &#40;&#41; 資源記錄新增至同盟伺服器的公司 DNS](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>將同盟伺服器的 IP 位址新增至 hosts 檔案  
為了讓同盟伺服器 proxy 可以在帳戶夥伴的周邊網路中如預期般運作，您必須在該同盟伺服器 proxy 上的 hosts 檔案中新增一個專案，指向同盟伺服器的 DNS 主機名稱，例如 \( fs.fabrikam.com \) 和 IP 位址 \( ，例如， \) 在帳戶夥伴的公司網路中192.168.1.4。 將此專案新增至 hosts 檔案，可防止同盟伺服器 proxy 自行聯繫，以解析用戶端對 \- 帳戶夥伴中的同盟伺服器起始的呼叫。  
  
若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱在[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>將同盟伺服器的 IP 位址新增至 hosts 檔案  
  
1.  流覽至% systemroot% \\ Winnt \\ System32 \\ 驅動程式目錄資料夾，並尋找**hosts**檔案。  
  
2.  啟動記事本，然後開啟 **hosts** 檔案。  
  
3.  將帳戶夥伴中同盟伺服器的 IP 位址和主機名稱新增至**hosts**檔案，如下列範例所示：  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  儲存並關閉檔案。  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>將主機 \( a \) 資源記錄新增至同盟伺服器 proxy 的周邊 DNS  
因此，網際網路上的用戶端可以透過新部署的同盟伺服器 proxy 成功存取同盟伺服器，您必須先 \( \) 在周邊 DNS 中建立主機 a 資源記錄。 此資源記錄會解析帳戶同盟伺服器的主機名稱 \( ，例如，fs.fabrikam.com \) 至帳戶同盟伺服器 PROXY 的 IP 位址， \( 例如 \) 周邊網路中的131.107.27.68。  
  
> [!NOTE]  
> 假設您使用的 DNS 伺服器執行 Windows 2000 Server、Windows Server 2003 或 Windows Server 2008 與 DNS 伺服器服務，以控制周邊 DNS 區域。  
  
若要完成此程式，至少需要**Administrators**的成員資格或同等許可權。  請參閱在[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>將主機 \( a \) 資源記錄新增至同盟伺服器 proxy 的周邊 DNS  
  
1.  在周邊網路的 DNS 伺服器上，開啟 [DNS] 嵌入式管理單元 \- 。 按一下 [開始]**** 並指向 [系統管理工具]****，然後按 [DNS]****。  
  
2.  在主控台樹中，以滑鼠右鍵 \- 按一下適用的正向對應區域，然後按一下 [**新增主機 \( A] 或 [AAAA \) **]。  
  
3.  在 [**名稱**] 中，只輸入同盟伺服器的電腦名稱稱。 例如，針對完整功能變數名稱 \( FQDN \) fs.fabrikam.com，輸入**fs**。  
  
4.  在 [ **ip 位址**] 中，輸入新的同盟伺服器 PROXY 的 IP 位址，例如**131.107.27.68**。  
  
5.  按一下 [新增主機]****。  
  
## <a name="additional-references"></a>其他參考  
[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[同盟伺服器 Proxy 的名稱解析需求](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807055(v=ws.11))  
  

---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: 在僅適用於周邊網路的 DNS 區域中設定同盟伺服器 Proxy 的名稱解析
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: de4627f2e03e6432f4e678cd9ca932819cb483d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408437"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>在僅適用於周邊網路的 DNS 區域中設定同盟伺服器 Proxy 的名稱解析


如此一來，Active Directory 同盟服務 \(AD FS @ no__t-1 案例中的同盟伺服器，名稱解析可以順利運作，其中一或多個網域名稱系統 \(DNS @ no__t-3 區域僅服務周邊網路，如下所示工作必須完成：  
  
-   必須更新同盟伺服器 proxy 上的 hosts 檔案，才能新增同盟伺服器的 IP 位址。  
  
-   周邊網路中的 DNS 必須設定為將 AD FS 主機名稱的所有用戶端要求，解析為同盟伺服器 proxy。 若要這麼做，請將主機 \(A @ no__t-1 資源記錄新增至同盟伺服器 proxy 的周邊 DNS。  
  
> [!NOTE]  
> 這些程式假設已在公司網路 DNS 中建立同盟伺服器的主機 @no__t 0A @ no__t-1 資源記錄。 如果此記錄尚未存在，請建立此記錄，然後執行這些程式。 如需有關如何為同盟伺服器建立主機 \(A @ no__t-1 資源記錄的詳細資訊，請參閱[將&#40;主機 a&#41;資源記錄新增至同盟伺服器的公司 DNS](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>將同盟伺服器的 IP 位址新增至 hosts 檔案  
如此一來，同盟伺服器 proxy 就可以在帳戶夥伴的周邊網路中如預期般運作。在帳戶夥伴的公司網路中，您必須將專案新增至該同盟伺服器 proxy 上的主機檔案中，指向同盟伺服器的 DNS 主機名稱 \(for 範例、no__t，以及 IP 位址 \(for 範例、192.168.1.4 @ no__t-3。 將此專案新增至 hosts 檔案，可防止同盟伺服器 proxy 自行聯繫，以將用戶端 @ no__t-0initiated 呼叫解析為帳戶夥伴中的同盟伺服器。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>將同盟伺服器的 IP 位址新增至 hosts 檔案  
  
1.  流覽至% systemroot% \\Winnt @ no__t-1System32 @ no__t-2Drivers directory 資料夾，並找出**hosts**檔案。  
  
2.  開啟 [記事本]，然後開啟 **[主機]** 檔案。  
  
3.  將帳戶夥伴中同盟伺服器的 IP 位址和主機名稱新增至**hosts**檔案，如下列範例所示：  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  儲存並關閉檔案。  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>將主機 \(A @ no__t-1 資源記錄新增至同盟伺服器 proxy 的周邊 DNS  
因此，網際網路上的用戶端可以透過新部署的同盟伺服器 proxy 成功存取同盟伺服器，您必須先在周邊 DNS 中建立主機 \(A @ no__t-1 資源記錄。 此資源記錄會將帳戶同盟伺服器的主機名稱 \(for 範例，fs. fabrikam .com @ no__t-1 解析為帳戶同盟伺服器 proxy 的 IP 位址，\(for 範例中為周邊網路中的 131.107.27.68 @ no__t-3。  
  
> [!NOTE]  
> 假設您使用的 DNS 伺服器執行 Windows 2000 Server、Windows Server 2003 或 Windows Server 2008 與 DNS 伺服器服務，以控制周邊 DNS 區域。  
  
若要完成此程式，至少需要**Administrators**的成員資格或同等許可權。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>若要將主機 \(A @ no__t-1 資源記錄新增至同盟伺服器 proxy 的周邊 DNS  
  
1.  在周邊網路的 DNS 伺服器上，開啟 DNS snap @ no__t-0in。 按一下 [**開始**]，指向 [系統**管理工具**]，然後按一下 [ **DNS**]。  
  
2.  在主控台樹中，右 @ no__t-0click 適用的正向對應區域，然後按一下 [**新增主機] \(a 或 AAAA @ no__t-3**。  
  
3.  在 [**名稱**] 中，只輸入同盟伺服器的電腦名稱稱。 例如，針對完整功能變數名稱 \(FQDN @ no__t-1 fs.fabrikam.com，輸入**fs**。  
  
4.  在 [ **ip 位址**] 中，輸入新的同盟伺服器 PROXY 的 IP 位址，例如**131.107.27.68**。  
  
5.  按一下 [新增主機]。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[同盟伺服器 Proxy 的名稱解析需求](https://technet.microsoft.com/library/dd807055.aspx)  
  


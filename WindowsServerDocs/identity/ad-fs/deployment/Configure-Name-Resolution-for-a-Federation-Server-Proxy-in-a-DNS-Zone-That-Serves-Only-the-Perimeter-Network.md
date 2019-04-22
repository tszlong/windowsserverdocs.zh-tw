---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: 在僅適用於周邊網路的 DNS 區域中設定同盟伺服器 Proxy 的名稱解析
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32b8e3cc133ce95872881115608bb8cfb17b2427
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816009"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>在僅適用於周邊網路的 DNS 區域中設定同盟伺服器 Proxy 的名稱解析

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

以便在 Active Directory 同盟服務的同盟伺服器的名稱解析可以順利運作\(AD FS\)的一或多個網域名稱系統中的案例\(DNS\)區域提供只周邊網路網路，下列必須完成工作：  
  
-   新增同盟伺服器的 IP 位址，必須更新同盟伺服器 proxy 上的主機檔案。  
  
-   周邊網路中的 DNS 必須解決所有的用戶端要求，AD FS 的主機名稱的同盟伺服器 proxy 設定。 若要這樣做，您將主機新增\(A\)同盟伺服器 proxy 的周邊 DNS 資源記錄。  
  
> [!NOTE]  
> 這些程序假設主機\(A\)資源記錄的同盟伺服器已建立在公司網路 DNS。 如果尚未存在此記錄，建立這筆記錄，並再執行這些程序。 如需有關如何建立主機\(A\)資源記錄，同盟伺服器，請參閱[將主機新增&#40;的&#41;至同盟伺服器的公司 DNS 資源記錄](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>將同盟伺服器的 IP 位址新增至主機檔案  
如此的同盟伺服器 proxy 能如預期般在帳戶夥伴的周邊網路中，您必須新增項目指向同盟伺服器的 DNS 主機名稱的同盟伺服器 proxy 上的主機檔案\(如 fs.fabrikam.com\)和 IP 位址\(例如 192.168.1.4\)帳戶夥伴的公司網路中。 將此項目新增至主機檔案防止同盟伺服器 proxy 本身，以解析用戶端連絡\-起始呼叫在帳戶夥伴的同盟伺服器。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>若要將同盟伺服器的 IP 位址新增至主機檔案  
  
1.  瀏覽至 %systemroot%\\Winnt\\System32\\驅動程式的目錄資料夾並找出**主機**檔案。  
  
2.  開啟 [記事本]，然後開啟 **[主機]** 檔案。  
  
3.  新增 IP 位址和同盟伺服器的主機名稱在帳戶夥伴**主機**檔案，如下列範例所示：  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  儲存並關閉檔案。  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>將主機新增\(A\)同盟伺服器 proxy 的周邊 DNS 資源記錄  
以便在網際網路上的用戶端成功可以存取同盟伺服器，透過新部署的同盟伺服器 proxy，您必須先建立一個主機\(A\)中的周邊 DNS 資源記錄。 此資源記錄的主機名稱解析 account federation server\(例如，fs.fabrikam.com\)帳戶同盟伺服器 proxy 的 IP 位址\(比方說，131.107.27.68\)中周邊網路。  
  
> [!NOTE]  
> 它會假設您使用 DNS 伺服器，執行 Windows 2000 Server、 Windows Server 2003 或 Windows Server 2008 DNS 伺服器服務，來控制周邊 DNS 區域。  
  
中的成員資格**系統管理員**，或同等權限，才能完成此程序的最小值。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>若要新增的主機\(A\)同盟伺服器 proxy 的周邊 DNS 資源記錄  
  
1.  在周邊網路的 DNS 伺服器，開啟 [DNS] 嵌入式管理單元\-中。 按一下 **開始**，指向**系統管理工具**，然後按一下**DNS**。  
  
2.  在主控台樹狀目錄中，以滑鼠右鍵\-按一下適用的正向對應區域，然後按一下**新主機\(A 或 AAAA\)**。  
  
3.  在 **名稱**，輸入只有同盟伺服器的電腦名稱。 例如，若為完整的網域名稱\(FQDN\) fs.fabrikam.com，輸入**fs**。  
  
4.  在  **IP 位址**，比方說，輸入新的同盟伺服器 proxy 的 IP 位址**131.107.27.68**。  
  
5.  按一下 [新增主機] 。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[同盟伺服器 Proxy 的名稱解析需求](https://technet.microsoft.com/library/dd807055.aspx)  
  


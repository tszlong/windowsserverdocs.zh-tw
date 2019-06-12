---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: 僅適用於周邊網路及網際網路用戶端的 DNS 區域中設定同盟伺服器 Proxy 的名稱解析
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: cef87e725db7068ac4ed93524e09a25de95ec276
ms.sourcegitcommit: 9a4ab3a0d00b06ff16173aed616624c857589459
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/11/2019
ms.locfileid: "66828512"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-both-the-perimeter-network-and-internet-clients"></a>僅適用於周邊網路及網際網路用戶端的 DNS 區域中設定同盟伺服器 Proxy 的名稱解析


以便在 Active Directory Federation Services 中的同盟伺服器 proxy 的名稱解析可以順利運作\(AD FS\)的一或多個網域名稱系統中的案例\(DNS\)區域提供兩者周邊網路和網際網路用戶端，必須先完成下列工作：  
  
-   若要解決所有的網際網路用戶端要求，AD FS 的主機名稱的同盟伺服器 proxy，必須設定您控制 [網際網路] 區域中的 DNS。 若要這麼做，您將主機新增\(A\)同盟伺服器 proxy 的網際網路 DNS 區域資源記錄。  
  
-   若要解決所有的連入用戶端要求，AD FS 的主機名稱的同盟伺服器，必須設定周邊網路中的 DNS。 若要這麼做，您將主機新增\(A\)同盟伺服器 proxy 的周邊 DNS 區域資源記錄。  
  
> [!NOTE]  
> 它假設主機\(A\)資源記錄的同盟伺服器已建立在公司網路 DNS。 如果尚未存在此記錄，建立這筆記錄，並再執行這些程序。 如需有關如何建立主機\(A\)資源記錄，同盟伺服器，請參閱[將主機新增&#40;的&#41;至同盟伺服器的公司 DNS 資源記錄](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>將主機新增\(A\)同盟伺服器 proxy 的網際網路 DNS 區域資源記錄  
讓網際網路上的用戶端電腦成功可以存取同盟伺服器，透過新部署的同盟伺服器 proxy，您必須先建立一個主機\(A\)中您所控制的網際網路 DNS 區域資源記錄。 此資源記錄的主機名稱解析 account federation server\(例如，fs.fabrikam.com\)帳戶同盟伺服器 proxy 的 IP 位址\(比方說，131.107.27.68\)中周邊網路。  
  
> [!NOTE]  
> 它會假設您使用執行 Windows 2000 Server、 Windows Server 2003 或 Windows Server 2008 的 DNS 伺服器服務的 DNS 伺服器來控制的網際網路 DNS 區域。  
  
中的成員資格**系統管理員**，或同等權限，才能完成此程序的最小值。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>若要新增的主機\(A\)同盟伺服器 proxy 的網際網路 DNS 區域資源記錄  
  
1.  在網際網路 DNS 區域的 DNS 伺服器，開啟 [DNS] 嵌入式管理單元\-中。  
  
2.  在主控台樹狀目錄中，以滑鼠右鍵\-按一下適用的正向對應區域，然後按一下**新主機\(A 或 AAAA\)** 。  
  
3.  在 **名稱**，輸入只有同盟伺服器的電腦名稱。 例如，若為完整的網域名稱\(FQDN\) fs.fabrikam.com，輸入**fs**。  
  
4.  在  **IP 位址**，為新同盟伺服器 proxy，比方說，131.107.27.68 輸入的 IP 位址。  
  
5.  按一下 [新增主機]  。  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>將主機新增\(A\)同盟伺服器 proxy 的周邊 DNS 區域資源記錄  
讓網際網路用戶端要求可以順利處理的同盟伺服器 proxy，並連線至同盟伺服器，它們會解析網際網路 DNS 區域之後，您必須建立一個主機\(A\)周邊網路中的資源記錄DNS 區域。 此資源記錄的主機名稱解析 account federation server\(比方說，fs。 fabrikam.com\)帳戶同盟伺服器的 IP 位址\(例如 192.168.1.4\)公司網路中。  
  
> [!NOTE]  
> 它會假設您使用執行 Windows 2000 Server、 Windows Server 2003、 Windows Server 2008 或 Windows Server® 2012年與 DNS 伺服器服務的 DNS 伺服器來控制周邊 DNS 區域。  
  
中的成員資格**系統管理員**，或同等權限，才能完成此程序的最小值。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>若要新增的主機\(A\)同盟伺服器 proxy 的周邊 DNS 區域資源記錄  
  
1.  在周邊網路的 DNS 伺服器，開啟**DNS 嵌入式管理單元\-在**。  
  
2.  在主控台樹狀目錄中，以滑鼠右鍵\-按一下適用的正向對應區域，然後按一下**新主機\(A 或 AAAA\)** 。  
  
3.  在 **名稱**，輸入只有同盟伺服器的電腦名稱。 例如，針對 FQDN fs.fabrikam.com，輸入 **fs**。  
  
4.  在  **IP 位址**文字方塊中，輸入 IP 位址在公司網路中，同盟伺服器，例如 192.168.1.4。  
  
5.  按一下 [新增主機]  。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[同盟伺服器 Proxy 的名稱解析需求](https://technet.microsoft.com/library/dd807055.aspx)  
  


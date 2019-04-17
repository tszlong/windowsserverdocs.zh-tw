---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: "設定名稱解析為聯盟 dns 伺服器 Proxy 區域該服務，這兩個周邊網路和網際網路戶端"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3b154459163b2142ff1d3aba424a86305d093de4
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-both-the-perimeter-network-and-internet-clients"></a>設定名稱解析為聯盟 dns 伺服器 Proxy 區域該服務，這兩個周邊網路和網際網路戶端

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

這樣的名稱解析可以成功適用於聯盟伺服器 proxy 一或多個 \(DNS\) 網域名稱系統區域做周邊網路和網際網路戶端 Active Directory 同盟服務 \(AD FS\) 案例中，必須完成以下工作：  
  
-   您可以控制網際網路區域中的 DNS 必須設定為解析所有網際網路 client 要求 AD fs 都主機聯盟 proxy 伺服器的名稱。 若要完成此動作，您加入主機 \(A\) 資源記錄網際網路 DNS 區域聯盟 proxy 伺服器。  
  
-   必須設定周邊網路的 DNS 解析所有連 client 要求 AD fs 主機聯盟伺服器的名稱。 若要完成此動作，您加入主機 \(A\) 資源記錄周邊 DNS 區域聯盟 proxy 伺服器。  
  
> [!NOTE]  
> 假設的主機 \(A\) 資源建立一筆資料聯盟伺服器已被 DNS 公司網路中。 如果這個記錄還不存在，建立這個記錄，然後執行下列程序。 如需有關如何來建立主機 \(A\) 資源聯盟伺服器的資訊，請查看[新增主機和 #40;A 與 #41;企業的 DNS 伺服器聯盟資源記錄](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>新增網際網路 DNS 時區主機 \(A\) 資源記錄聯盟 proxy 伺服器  
這樣 client 網際網路上的電腦已成功可以存取聯盟伺服器透過部署新聯盟 proxy 伺服器，您必須先建立主機 \(A\) 資源記錄您控制網際網路 DNS 區域。 此資源記錄解析主機伺服器的名稱 account 聯盟 \ (例如，fs.fabrikam.com\) proxy account 聯盟伺服器的 IP 位址 \ (例如，131.107.27.68\) 周邊網路中。  
  
> [!NOTE]  
> 假設您使用執行 Windows 2000 Server、Windows Server 2003 或 Windows Server 2008 的 DNS 伺服器服務的 DNS 伺服器控制網際網路 DNS 區域。  
  
資格在**系統管理員**，或相當於，才能完成此程序最小值。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](http://go.microsoft.com/fwlink/?LinkId=83477)。   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>新增網際網路 DNS 時區主機 \(A\) 資源記錄聯盟 proxy 伺服器  
  
1.  在 [網際網路 DNS 區域的 DNS 伺服器，開放 DNS snap\ 中。  
  
2.  在主機中樹狀結構 right\ 按一下適用的正向對應區域，，然後按一下**新主機 \(A or AAAA\)**。  
  
3.  在**名稱**，輸入只聯盟伺服器的電腦名稱。 例如的完整網域名稱 \(FQDN\) fs.fabrikam.com，輸入**fs**。  
  
4.  在**的 IP 位址**，輸入新聯盟伺服器 proxy，例如 131.107.27.68 的 IP 位址。  
  
5.  按一下**新增主機**。  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>新增至周邊 DNS 區域主機 \(A\) 資源記錄聯盟 proxy 伺服器  
可讓網際網路 client 要求處理聯盟 proxy 伺服器成功並到達聯盟伺服器網際網路 DNS 區域解析之後，您必須建立主機 \(A\) 資源記錄周邊 DNS 區域中。 此資源記錄解析主機伺服器的名稱 account 聯盟 \ (例如 fs。 fabrikam.com\) account 聯盟伺服器的 IP 位址 \ (例如，192.168.1.4\) 公司網路中。  
  
> [!NOTE]  
> 假設您使用執行 Windows 2000 Server、Windows Server 2003、Windows Server 2008 或 Windows Server® 2012 年的 DNS 伺服器服務的 DNS 伺服器控制周邊 DNS 區域。  
  
資格在**系統管理員**，或相當於，才能完成此程序最小值。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](http://go.microsoft.com/fwlink/?LinkId=83477)。   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>若要主機 \(A\) 資源記錄周邊 DNS 時區新增聯盟 proxy 伺服器  
  
1.  在周邊網路的 DNS 伺服器，請打開**DNS snap\ 在**。  
  
2.  在主機中樹狀結構 right\ 按一下適用的正向對應區域，，然後按一下**新主機 \(A or AAAA\)**。  
  
3.  在**名稱**，輸入只聯盟伺服器的電腦名稱。 例如，fqdn fs.fabrikam.com 中，輸入**fs**。  
  
4.  在**的 IP 位址**文字方塊中，輸入 IP 位址的企業網路，在聯盟伺服器，例如 192.168.1.4。  
  
5.  按一下**新增主機**。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單︰ 聯盟 Proxy 伺服器設定](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[聯盟的 Proxy 伺服器的名稱解析需求](https://technet.microsoft.com/library/dd807055.aspx)  
  


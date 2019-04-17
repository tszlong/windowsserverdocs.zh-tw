---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: "聯盟伺服器 Proxy 做周邊網路 DNS 區域中的設定的名稱解析"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32b8e3cc133ce95872881115608bb8cfb17b2427
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>聯盟伺服器 Proxy 做周邊網路 DNS 區域中的設定的名稱解析

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

這樣的名稱解析可以順利聯盟伺服器一或多個網域名稱系統 \(DNS\) 區提供服務周邊網路 Active Directory 同盟服務 \(AD FS\) 案例中，必須完成以下工作：  
  
-   必須更新主機上的檔案聯盟 proxy 伺服器新增聯盟伺服器的 IP 位址。  
  
-   必須設定周邊網路的 DNS 解析所有 AD FS client 要求主機聯盟 proxy 伺服器的名稱。 若要這樣做，您加入主機 \(A\) 資源記錄周邊 DNS 聯盟 proxy 伺服器。  
  
> [!NOTE]  
> 下列程序假設的主機 \(A\) 資源建立一筆資料聯盟伺服器已被 DNS 公司網路中。 如果這個記錄還不存在，建立這個記錄，，然後執行下列程序。 如需如何建立主機 \(A\) 資源記錄聯盟伺服器，查看[新增主機和 #40;A 與 #41;企業的 DNS 伺服器聯盟資源記錄](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>聯盟伺服器的 IP 位址新增到主控檔案  
使聯盟 proxy 伺服器能如預期般 account 協力廠商周邊網路中，您必須將項目新增至主機上的檔案聯盟伺服器 proxy 指向聯盟伺服器的主機的 DNS 名稱 \ (例如，fs.fabrikam.com\) 及 IP 位址 \ (例如，192.168.1.4\) 中的 account 合作夥伴公司網路。 將此項目新增至主機的檔案會防止聯盟 proxy 伺服器連絡本身解析 client\ 車載機起始呼叫到聯盟 account 合作夥伴的伺服器。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>若要新增聯盟伺服器的 IP 位址主控檔案  
  
1.  瀏覽到 %systemroot%\\Winnt\\System32\\Drivers directory 資料夾，並找出**主機**檔案。  
  
2.  在「記事本」，[開始]，然後打開**主機**檔案。  
  
3.  在 [account 合作夥伴中加入的 IP 位址和主機聯盟伺服器的名稱**主機**檔案，以下的範例所示：  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  儲存，並關閉檔案。  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>新增至周邊 DNS 主機 \(A\) 資源記錄聯盟 proxy 伺服器  
這樣戶端在網際網路上已成功可以存取聯盟伺服器透過部署新聯盟 proxy 伺服器，您必須先建立主機 \(A\) 資源記錄 DNS 周邊設備中。 此資源記錄解析主機伺服器的名稱 account 聯盟 \ (例如，fs.fabrikam.com\) proxy account 聯盟伺服器的 IP 位址 \ (例如，131.107.27.68\) 周邊網路中。  
  
> [!NOTE]  
> 假設您使用 DNS 伺服器，使用 DNS 伺服器，服務執行的 Windows 2000 Server、Windows Server 2003 或 Windows Server 2008 控制周邊 DNS 區域。  
  
資格在**系統管理員**，或相當於，才能完成此程序最小值。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>新增至周邊 DNS 主機 \(A\) 資源記錄聯盟 proxy 伺服器  
  
1.  在周邊網路的 DNS 伺服器，開放 DNS snap\ 中。 按一下**[開始]**，指向 [**系統管理工具]**，然後按一下 [ **DNS**。  
  
2.  在主機中樹狀結構 right\ 按一下適用的正向對應區域，，然後按一下**新主機 \(A or AAAA\)**。  
  
3.  在**名稱**，輸入只聯盟伺服器的電腦名稱。 例如的完整網域名稱 \(FQDN\) fs.fabrikam.com，輸入**fs**。  
  
4.  在**的 IP 位址**，例如，輸入 IP 位址新聯盟伺服器 proxy **131.107.27.68**。  
  
5.  按一下**新增主機**。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單︰ 聯盟 Proxy 伺服器設定](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[聯盟的 Proxy 伺服器的名稱解析需求](https://technet.microsoft.com/library/dd807055.aspx)  
  


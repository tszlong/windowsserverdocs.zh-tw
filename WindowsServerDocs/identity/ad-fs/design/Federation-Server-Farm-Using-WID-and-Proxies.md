---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: "使用 WID 與 Proxy 聯盟伺服器陣列"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e372f066fc82b9857d438234b491732a177e24fa
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>使用 WID 與 Proxy 聯盟伺服器陣列

>適用於：Windows Server 2016、Windows Server 2012 R2

這個 Active Directory 同盟服務 \(AD FS\) 部署拓撲聯盟伺服器發電廠相同的 Windows 內部資料庫 \(WID\) 拓撲，但加入支援外部使用者周邊網路 proxy 電腦。 這些 proxy 重新導向來自您的企業網路外聯盟伺服器陣列 client 驗證要求。 AD FS 舊版，在這些 proxy 被稱為聯盟的 proxy 伺服器。  
  
> [!IMPORTANT]  
> 在 Active Directory 同盟服務 \(AD FS\) 在 Windows Server 2012 R2，聯盟 proxy 伺服器的角色被處理呼叫 Web 應用程式 Proxy 新遠端存取的角色服務。 若要可讓您的企業網路，是目的部署聯盟伺服器 proxy 在舊版 AD FS，例如 AD FS 2.0 和在 Windows Server 2012，AD FS 以外的協助工具的 AD FS 您可以將一或多個 web 應用程式 proxy AD fs 在 Windows Server 2012 R2 的部署。  
>   
> AD FS 處在，Web 應用程式 Proxy AD FS 聯盟伺服器 proxy 功能。 除了，Web 應用程式 Proxy 提供 web 應用程式在您的企業網路，讓使用者在任何裝置上存取它們的以外的公司網路 proxy 反向功能。 如需詳細資訊，有關 Web 應用程式 Proxy 角色服務，Web 應用程式 Proxy 概觀。  
>   
> 計劃的 proxy Web 應用程式部署，您可以檢視的下列主題中的資訊：  
>   
> -   [規劃 Web 應用程式 Proxy 基礎結構 (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [計畫網站的應用程式的 Proxy 伺服器](https://technet.microsoft.com/library/dn383647.aspx)  
  
## <a name="deployment-considerations"></a>部署注意事項  
本節各種考量有關的目標對象、優點和這部署拓撲相關聯的限制。  
  
### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？  
  
-   100 或較少設定的信任關係需要他們內部使用者和外部使用者提供的組織 \（誰登入電腦，實際上以外的公司 network\）的單一 sign\ 上 \(SSO\) 存取聯盟應用程式或服務  
  
-   組織必須為其內部使用者和外部使用者提供 SSO 存取與 Microsoft Office 365  
  
-   較小的組織外部使用者且需要備援可縮放服務  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用這個拓撲的好處為何？  
  
-   相同好處所列的[聯盟伺服器發電廠使用 WID](Federation-Server-Farm-Using-WID.md)拓撲，再加上的優點外部使用者提供額外的存取  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用這個拓撲限制為何？  
  
-   限制相同為列出適用於[聯盟伺服器發電廠使用 WID](Federation-Server-Farm-Using-WID.md)拓撲  

||1 \-100 資源點數信任|超過 100 資源點數信任 
| ----- |-----| ------ |
|1 \-30 AD FS 節點|WID 支援|不支援使用 WID \-SQL 需要 
|超過 30 AD FS 節點|不支援使用 WID \-SQL 需要|不支援使用 WID \-SQL 需要  
  
## <a name="server-placement-and-network-layout-recommendations"></a>伺服器配置建議位置與網路  
若要部署這個拓撲，除了新增兩個 web 應用程式 proxy，您必須確定周邊網路，也可以提供及存取權的網域名稱系統 \(DNS\) 伺服器第二個網路負載平衡 \(NLB\) 主機。 必須設定使用 Internet\ 無障礙叢集 IP 位址，NLB 叢集 NLB 第二部主機和必須使用先前 NLB 叢集您企業網路 \(fs.fabrikam.com\) 上設定為相同的叢集 DNS 名稱設定。 Web 應用程式 proxy 也應該 Internet\ 存取 IP 位址設定。  
  
下圖顯示現有聯盟伺服器陣列與 WID 拓撲之前所述的方式虛構 Fabrikam，Inc.公司提供存取周邊 DNS 伺服器，將新增第二部具有相同叢集 DNS 名稱 \(fs.fabrikam.com\)，NLB 主機，並將有兩個 web 應用程式 proxy \(wap1 and wap2\) 周邊網路。  
  
![WID 農場 Proxy](media/WIDFarmADFSBlue.gif)  
  
如需有關如何聯盟伺服器或網路應用程式的 proxy 設定使用您的網路環境，查看 [名稱解析需求」一節中[AD FS 需求](AD-FS-Requirements.md)和[計劃 Web 應用程式 Proxy 基礎結構 (WAP)](https://technet.microsoft.com/library/dn383648.aspx)。  
  
## <a name="see-also"></a>也了  
[AD FS 部署拓撲計劃](Plan-Your-AD-FS-Deployment-Topology.md)  
[在 Windows Server 2012 R2 的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  


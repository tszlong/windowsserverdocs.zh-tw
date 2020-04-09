---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: 使用 SQL Server 的同盟伺服器陣列
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 862cbc74833e2d4e9f385ba961b58a1f703e6611
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853131"
---
# <a name="federation-server-farm-using-sql-server"></a>使用 SQL Server 的同盟伺服器陣列

Active Directory 同盟服務 \(AD FS\) 的此拓撲與使用 Windows Internal Database \(WID\) 部署拓撲的同盟伺服器陣列不同，因為它不會將資料複寫到伺服器陣列中的每部同盟伺服器。 取而代之的是，伺服器陣列中的所有同盟伺服器都可以讀取資料，並將其寫入至儲存在執行 Microsoft SQL Server 的伺服器上的通用資料庫。  
  
## <a name="deployment-considerations"></a>部署考量  
本節說明與此部署拓撲相關聯的目標物件、優點和限制的各種考慮。  
  
### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？  
  
-   具有超過100信任關係的大型組織，必須提供內部使用者和外部使用者 \(SSO\) 存取同盟應用程式或服務的\-  
  
-   已經使用 SQL Server，而且想要利用現有工具和專業知識的組織  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓撲的優點有哪些？  
  
-   支援較大量的信任關係 \(超過 100\)  
  
-   支援權杖重新執行偵測 \(安全性功能\) 和成品解析 \(安全性聲明標記語言 \(SAML\) 2.0 通訊協定的一部分\)  
  
-   支援 SQL Server 的完整優點，例如資料庫鏡像、容錯移轉叢集、報告及管理工具  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓撲的限制為何？  
  
-   根據預設，此拓撲不會提供資料庫冗余。 雖然具有 WID 拓撲的同盟伺服器陣列會自動複寫伺服器陣列中每部同盟伺服器上的 WID 資料庫，但具有 SQL Server 拓撲的同盟伺服器陣列只包含一個資料庫複本  
  
> [!NOTE]  
> SQL Server 支援許多不同的資料和應用程式冗余選項，包括容錯移轉叢集、資料庫鏡像，以及數種不同類型的 SQL Server 複寫。  
  
Microsoft 資訊技術 \(IT\) 部門使用高\-安全的 SQL Server 資料庫鏡像 \(同步\) 模式和容錯移轉叢集，以提供\-實例的高 SQL Server 可用性支援。 SQL Server 交易 \(對等\-，以\-對等\) 和合併式複寫尚未由 Microsoft 的 AD FS 產品小組測試。 如需 SQL Server 的詳細資訊，請參閱[高可用性解決方案總覽](https://go.microsoft.com/fwlink/?LinkId=179853)或[選取適當的複寫類型](https://go.microsoft.com/fwlink/?LinkId=214648)。  
  
### <a name="supported-sql-server-versions"></a>支援的 SQL Server 版本  
與 Windows Server 2012 一起安裝的 AD FS 支援下列 SQL server 版本：  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
## <a name="server-placement-and-network-layout-recommendations"></a>伺服器放置和網路設定建議  
與具有 WID 拓撲的同盟伺服器陣列類似，伺服器陣列中的所有同盟伺服器都會設定為使用一個叢集網域名稱系統 \(DNS\) 名稱 \(代表同盟服務名稱\) 和一個叢集 IP 位址做為網路負載平衡 \(NLB\) 叢集設定的一部分。 這可協助 NLB 主機將用戶端要求配置給個別的同盟伺服器。 同盟伺服器 proxy 可用來將用戶端要求傳送至同盟伺服器陣列。  
  
下圖顯示虛構的 Contoso 製藥公司如何將其同盟伺服器陣列與公司網路中的 SQL Server 拓撲一起部署。 它也會顯示該公司如何設定具有 DNS 伺服器存取權的周邊網路、使用相同叢集 DNS 名稱的額外 NLB 主機 \(fs.contoso.com\) 用於公司網路 NLB 叢集，以及兩個同盟伺服器 proxy \(fsp1 和 fsp2 新增\)。  
  
![使用 SQL 的伺服器陣列](media/FarmSQLProxies.gif)  
  
如需有關如何設定網路環境以與同盟伺服器或同盟伺服器 proxy 搭配使用的詳細資訊，請參閱同盟伺服器的[名稱解析需求](Name-Resolution-Requirements-for-Federation-Servers.md)或[同盟伺服器 Proxy 的名稱解析需求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

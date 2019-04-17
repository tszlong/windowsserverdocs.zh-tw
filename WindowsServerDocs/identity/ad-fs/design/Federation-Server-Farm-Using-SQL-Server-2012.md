---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: "使用 SQL Server 聯盟伺服器陣列"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a0fff975b9cb278e59686323d2bd72e641597573
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="federation-server-farm-using-sql-server"></a>使用 SQL Server 聯盟伺服器陣列

>適用於：Windows Server 2012

這個 Active Directory 同盟服務 \(AD FS\) 拓撲與它不會複寫發電廠每個聯盟伺服器資料，使用 Windows 內部資料庫 \(WID\) 部署拓撲聯盟伺服器陣列不同。 改所有聯盟伺服器可讀取和寫入通用資料庫會儲存在位於公司網路中的 Microsoft SQL server 的伺服器上的資料。  
  
## <a name="deployment-considerations"></a>部署注意事項  
本節各種考量有關的目標對象、優點和這部署拓撲相關聯的限制。  
  
### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？  
  
-   大型的組織超過 100 信任關係，必須為其內部使用者和外部使用者單一 sign\ 上 \(SSO\) 存取聯盟應用程式或服務提供使用  
  
-   組織已經使用 SQL Server，並且想要利用其現有的工具和專業  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用這個拓撲的好處為何？  
  
-   支援信任關係的數字越大 \(more than 100\)  
  
-   支援權杖重播偵測 \(a security feature\) 和成品解析度 \ (安全性判斷提示標記語言 \(SAML\) 2.0 的一部分 protocol\)  
  
-   支援 SQL Server 的完整優點資料庫鏡像、容錯、報告] 下方，和管理工具  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用這個拓撲限制為何？  
  
-   這個拓撲預設不提供資料庫冗餘。 聯盟伺服器陣列有 SQL Server 拓撲聯盟伺服器陣列有 WID 拓撲會自動複製 WID 資料庫陣列中每個聯盟伺服器上的，但包含份資料庫  
  
> [!NOTE]  
> SQL Server 支援許多不同的資料與應用程式冗餘選項，包括容錯，請稍後及 SQL Server 複寫數種不同類型。  
  
Microsoft 的資訊技術 \(IT\) 部門使用 SQL Server 資料庫鏡像 high\ 安全 \(synchronous\) 模式和容錯提供 high\ 可用性支援 SQL Server 執行個體。 Microsoft AD FS product 小組尚未經過測試 SQL Server 交易 \(peer\-to\-peer\) 及合併複寫。 如需 SQL Server 的詳細資訊，請查看[高可用性方案概觀](https://go.microsoft.com/fwlink/?LinkId=179853)或[選取適當的︰ 複寫輸入](https://go.microsoft.com/fwlink/?LinkId=214648)。  
  
### <a name="supported-sql-server-versions"></a>支援的 SQL Server 版本  
安裝 Windows Server 2012 AD FS 進行支援下列 SQL server 版本：  
  
-   SQL Server 2008 \ 日 R2  
  
-   SQL Server 2012  
  
## <a name="server-placement-and-network-layout-recommendations"></a>伺服器配置建議位置與網路  
類似的 WID 拓撲聯盟伺服器陣列聯盟伺服器的所有設定為使用一個叢集網域名稱系統 \(DNS\) 名稱 \（代表同盟服務 name\）和網路負載平衡 \(NLB\) 叢集組態的一部分一個叢集 IP 位址。 這可協助 NLB 主機配置 client 要求的個人聯盟伺服器。 聯盟伺服器 proxy 可用於 proxy 伺服器聯盟陣列 client 請求。  
  
下圖顯示虛構 Contoso 醫藥公司部署其聯盟具有伺服器陣列 SQL Server 拓撲公司網路中的方式。 它也示範如何該公司設定周邊網路存取權的 DNS 伺服器，使用適用於企業網路 NLB 叢集、相同叢集 DNS 名稱 \(fs.contoso.com\) 其他 NLB 主機與兩個聯盟伺服器 proxy \(fsp1 and fsp2\)。  
  
![使用 SQL server 陣列](media/FarmSQLProxies.gif)  
  
如需有關如何使用您的網路環境設定聯盟伺服器或聯盟的 proxy 伺服器，查看任一個[聯盟伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Servers.md)或[聯盟的 Proxy 伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

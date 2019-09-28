---
ms.assetid: 7013fc21-9ced-4f9d-9588-cb04d6d60924
title: 規劃同盟伺服器容量
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 418bc5d53a2bd11afa8563b07bbff76c89495715
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407976"
---
# <a name="planning-for-federation-server-capacity"></a>規劃同盟伺服器容量

同盟伺服器的容量規劃可協助您評估：  
  
-   哪些因素會增加 AD FS 設定資料庫的大小。  
  
-   每部同盟伺服器的適當硬體需求。  
  
-   要在每個組織中放置的同盟伺服器數目。  
  
同盟伺服器會將安全性權杖發行給使用者。 這些權杖會提供給信賴憑證者使用。 同盟伺服器會在驗證使用者之後，或在收到先前由夥伴同盟服務所發行的安全性權杖之後，發出安全性權杖。 當使用者一開始登入同盟應用程式時，或當他們的安全性權杖在存取同盟應用程式時過期時，會向同盟服務要求安全性權杖。  
  
同盟伺服器的設計目的是為了配合使用 Microsoft 網路負載平衡 \(NLB @ no__t-2 技術的高 @ no__t 0availability 伺服器伺服器陣列設定。 伺服器陣列設定中的同盟伺服器可以獨立服務要求，而不需要存取每個要求的任何一般伺服器陣列元件。 因此，向外延展同盟伺服器部署時，不會產生額外的負擔。  
  
**相關**  
  
-   針對任務 @ no__t-0critical 或 high @ no__t-1availability 部署，建議您在每個夥伴組織中建立一個小型同盟伺服器陣列，每個伺服器陣列至少要有兩部同盟伺服器，才能提供容錯能力。  
  
-   隨著高可用性和相應放大同盟伺服器的需求，相應放大是針對特定同盟服務處理每秒要求數高的建議方法。 除了本指南的基本設定外，擴充不太可能會產生大量的容量處理增益。  
  
## <a name="ad-fs-configuration-database-size-and-growth"></a>AD FS 設定資料庫大小和成長  
AD FS 設定資料庫的大小通常會被視為很小，而且資料庫大小不一定是 AD FS 部署的主要考慮。  AD FS 設定資料庫的確切大小，主要取決於信任關係的數目，以及相關聯的信任 @ no__t-0related 中繼資料，例如宣告、宣告規則，以及針對每個信任所設定的監視設定。 當設定資料庫中的信任專案數目增加時，就需要更多的磁碟空間。  
  
如需 AD FS 設定資料庫的其他部署資訊，請參閱[AD FS 部署拓撲考慮](AD-FS-Deployment-Topology-Considerations.md)。  
  
## <a name="memory-cpu-and-disk-space-requirements"></a>記憶體、CPU 和磁碟空間需求  
幸運的是，同盟伺服器的記憶體、CPU 和磁碟空間需求很低，而且不可能是硬體決策的驅動因素。 如需硬體需求的詳細資訊，請參閱 @no__t 0Appendix A：查看 AD FS 需求 @ no__t-0。  
  
> [!NOTE]  
> 在 AD FS 產品小組所執行的測試中，使用以專用 SQL Server 設定的同盟伺服器陣列來儲存 AD FS 設定資料庫時，SQL Server 比起的整體負載會較低。 在使用已設定為使用單一 SQL Server 之四個 @ no__t-0federation @ no__t-1server 伺服器陣列的一項測試中，即使在將同盟伺服器設為目標使用率的測試中，CPU 使用率也不會超過 10%。  
  
## <a name="bk_estimatefs"></a>估計貴組織的同盟伺服器數目  
為了簡化同盟伺服器的硬體規劃程式，AD FS 產品小組開發了 AD FS 容量規劃調整大小的試算表。 這份 Excel 試算表包含計算機 @ no__t-0like 功能，將會採用您提供給您組織中使用者的預期使用方式資料，並為您的 AD FS 生產環境傳回建議的最佳同盟伺服器數目。  
  
> [!NOTE]  
> 這份試算表建議的同盟伺服器數目，是根據 AD FS 產品小組在測試期間所使用的硬體和網路規格而定。 因此，您必須在此內容中瞭解試算表所建議的同盟伺服器數目。  如需測試期間所使用之規格的詳細資訊，請參閱標題[為 AD FS 伺服器容量規劃](Planning-for-AD-FS-Server-Capacity.md)主題。  
  
### <a name="using-the-ad-fs-capacity-planning-sizing-spreadsheet"></a>使用 AD FS 容量規劃調整大小試算表  
當您使用此試算表時，您必須選取一個值 \(either **40%** 、 **60%** 或**80%** \)，其最能代表您預期會在期間將驗證要求傳送至同盟伺服器的總使用者百分比尖峰使用期間。  
  
然後，您必須選取一個值 \(either **1 分鐘** **15 分鐘**，或**1 小時**的 \)，其最能代表您預期尖峰使用期間最後的時間長度。 例如，您可以將 40% 評估為在15分鐘內登入的使用者總數值，或 60% 的使用者將在1小時內登入。 這些值一起定義會計算大小調整建議的尖峰負載設定檔。  
  
接下來，您必須根據使用者是否為，指定需要對目標宣告 @ no__t 1aware 應用程式進行單一正負號 @ no__t 0on 存取的使用者總數：  
  
-   從實際連線到公司網路的本機電腦登入 Active Directory \(through Windows 整合式驗證 @ no__t-1  
  
-   從未實際連線到公司網路的電腦進行遠端登入 Active Directory \(through Windows 整合式驗證或使用者名稱和密碼 @ no__t-1  
  
-   來自另一個組織並嘗試從信任的夥伴存取目標宣告 @ no__t-0aware 應用程式  
  
-   從 SAML 2.0 識別提供者，並嘗試存取目標宣告 @ no__t-0aware 應用程式  
  
#### <a name="how-to-use-this-spreadsheet"></a>如何使用此試算表  
您可以針對您打算部署的每個同盟伺服器陣列實例使用下列步驟，以判斷建議的同盟伺服器數目。  
  
1.  下載並開啟[適用于 Windows server 2012 R2 的 AD FS 容量規劃調整大小試算表](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)，或[適用于 windows server 2016 的 AD FS 容量規劃調整大小試算表](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)。
  
2.  在**尖峰系統使用期間的右側儲存格中，我預期 [我的使用者要驗證的百分比**] 資料格，按一下資料格，然後使用 drop @ no__t-1down 箭號來選取您的預估系統使用率層級（ **40%** ）部署的**60%** 或**80%** 。  
  
3.  在**下列時間範圍內**的右邊儲存格中，按一下資料格，然後使用 drop @ no__t-1down 箭號選取**1 分鐘**、 **15 分鐘**或**1 小時**來選取尖峰負載的持續時間。  
  
4.  在 [**輸入估計的內部應用程式數目] 右邊的儲存格中 \(such 為 SharePoint \(2007 或 2010 @ no__t-3 或宣告感知 web 應用程式 @ no__t-4**資料格，輸入您將在中使用的內部應用程式數目結構.  
  
5.  在 [**輸入預估的線上應用程式數目] 右邊的儲存格中 @no__t 1such 為 [Office 365 Exchange online]、[SharePoint online] 或 [Lync online @ no__t-2** ] 資料格，輸入您將在組織中使用的線上應用程式或服務數目。  
  
6.  在標題為 [**使用者數目**] 的資料格底下，于套用至範例應用程式案例的每個資料列上輸入一個數位，您的使用者將需要單一登入 @ no__t 1on 存取權。 此資料行應該包含已定義的使用者數目，而不是每秒的尖峰使用者數。 如果對應用程式的存取嘗試必須先經過主領域探索頁面，請輸入**Y**。如果您不確定此選項，請輸入**Y**。  
  
7.  請檢查下列建議的值：  
  
    1.  如需建議的同盟伺服器總數，請參閱以灰色反白顯示的右下方資料格。  
  
    2.  針對每個範例應用程式案例建議的伺服器數目，請參閱以灰色反白顯示之資料列上的儲存格。  
  
> [!NOTE]  
> 將會在標題為 [**建議的同盟伺服器總數**] 資料格右邊的儲存格中自動計算的值，其中包含的公式會將額外的 20% 緩衝區新增至所有的總計其前面的每個個別資料列中的值。 新增到**同盟伺服器總數**的公式建議將此緩衝區中的資料格建立到您部署的同盟伺服器總數，讓伺服器陣列的整體負載不太可能達到其飽和點。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

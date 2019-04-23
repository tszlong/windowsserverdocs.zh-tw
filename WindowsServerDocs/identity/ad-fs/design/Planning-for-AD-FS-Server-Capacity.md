---
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: 規劃 AD FS 伺服器容量
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 484dd08edef85b91e777f8963f175a6172c75430
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847389"
---
# <a name="planning-for-ad-fs-server-capacity"></a>規劃 AD FS 伺服器容量

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

  
> [!NOTE]  
> 本主題中提供的內容不會反映實際測試在執行 Windows Server 2012 的伺服器上執行。 一旦執行必要的測試之後，即會更新這個主題。  
  
Active Directory Federation Services 的容量規劃\(AD FS\)程序會預測您的 Federation Service 尖峰使用量，以及規劃或調整\-AD FS 伺服器部署以符合這些載入需求。  
  
本章節描述同盟伺服器與同盟伺服器 proxy 角色的部署指引，並根據實驗室測試 Microsoft 的 AD FS 產品小組所執行的。 此內容的目的是要協助您：  
  
-   仔細評估您的組織特定 AD FS 部署，例如 AD FS 伺服器數目的硬體需求。  
  
-   精確地專案正負號的預期的尖峰使用量\-在要求中，請規劃成長，並確認 AD FS 部署能夠處理該預期的尖峰使用量。  
  
在您繼續閱讀這個容量規劃內容之前，建議您先依照以下兩個表格中所示的順序來完成工作。 在第一個表格中，我們提供建議工作的連結，以協助提供適用於這個容量規劃討論的相關內容。  
  
|建議的工作|描述|參考資料|  
|--------------------|---------------|-------------|  
|了解部署 AD FS 同盟伺服器和同盟伺服器 proxy 的需求|檢閱部署同盟伺服器和同盟伺服器 Proxy 所需的重要硬體和軟體需求。|[附錄 a:檢閱 AD FS 需求](Appendix-A--Reviewing-AD-FS-Requirements.md)|  
|選取您將部署在組織中的 AD FS 設定資料庫類型|使用本節中的容量規劃資料之前，您必須先判斷哪一個 AD FS 設定資料庫的類型將會部署，可能是 Windows 內部資料庫\(WID\)或結構化查詢語言\(SQL\)資料庫。|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md);<br /><br />[AD FS 部署拓撲考量](AD-FS-Deployment-Topology-Considerations.md)|  
|決定要和新的 AD FS 設定資料庫選項搭配使用的拓撲配置類型|一旦決定要在部署中使用的 AD FS 設定資料庫類型之後，就需要考量哪一個部署拓撲最符合您需要在實際執行環境內放置同盟伺服器和同盟伺服器 Proxy 的位置。|[判斷您的 AD FS 部署拓撲](Determine-Your-AD-FS-Deployment-Topology.md)|  
|了解主要 AD FS 相關容量規劃詞彙|檢閱常用容量規劃使用 AD FS 容量規劃討論的詞彙的定義。|請參閱本主題中標題為 [AD FS 容量規劃詞彙](Planning-for-AD-FS-Server-Capacity.md#bk_terms)的小節。|  
  
在檢閱上表的內容之後，您現在可以完成下表中的必要工作。  
  
|必要的工作|描述|參考資料|  
|---------------------|---------------|-------------|  
|下載 AD FS 容量規劃調整大小試算表|AD FS 容量規劃調整大小試算表可協助您判斷所需的 AD FS 同盟伺服器陣列部署的同盟伺服器數目。 以下提供的連結中有如何使用這個試算表的指示，可供第二個工作使用。|[AD FS 容量規劃試算表](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|  
|收集資料需要單一的正負號的使用者數目\-上\(SSO\)存取目標宣告\-感知應用程式和預期的尖峰使用量與這個存取相關聯|您收集的這個使用者資料將用於 AD FS 容量規劃調整大小試算表內所需的輸入值。|[評估貴組織的同盟伺服器的數目](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|  
|AD FS 容量規劃試算表適用於 Windows Server 2016|更新適用於 Windows Server 2016 的規劃工作表|[AD FS 的 Windows Server 2016 的容量規劃](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)  
  
## <a name="bk_terms"></a>AD FS 容量規劃詞彙  
下表描述這個容量規劃章節的 AD FS 設計指南中經常使用的重要詞彙。 如需的 AD FS 詞彙的更完整清單，請參閱 < [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)。  
  
|詞彙|定義|  
|--------|--------------|  
|並行使用者|在指定時段內 (通常是尖峰活動期間)，預期會提交要求給服務的使用者估計數目。|  
|作用中的使用者|在指定時段內，系統上處於作用中狀態但不一定會提交要求之使用者的大約平均數目。|  
|定義的使用者|理論上的使用者計數上限，通常會以已在系統中定義帳戶之使用者的數目為依據。|  
|每秒要求數目|可能是用戶端所提交的要求數目\(在談論系統上的負載\)或未處理的伺服器\(談論伺服器輸送量時\)中第二個。 規劃伺服器處理器和記憶體容量時會使用此衡量標準。|  
|目標伺服器的回應性與使用率|繫結可接受的伺服器效能範圍的成功衡量標準。 通常，如果回應性低於目標或使用率超過目標，便會將系統視為已超載且需更多容量。|  
|Windows 內部資料庫\(WID\)|可用來當做替代 SQL Server 中特定的 AD FS 部署預設的 AD FS 設定資料庫。|  
  
## <a name="configuration-environment-used-during-ad-fs-testing"></a>AD FS 測試期間使用的設定環境  
本節說明 AD FS 產品小組用來執行其測試設定環境。 小組會使用下列電腦硬體、軟體及網路設定，來蒐集測試同盟伺服器時的效能與延展性資料：  
  
-   雙四核心 2.27 ghz \(GHz\) \(8 個核心\)  
  
-   16\-GB RAM  
  
-   Windows Server 2008 R2 Enterprise Edition  
  
-   Gigabit 網路  
  
> [!NOTE]  
> 雖然在測試期間在同盟伺服器上使用 16 GB 的 RAM，更適合的記憶體大小，例如 4 GB 的 RAM，每一部同盟伺服器可以用於大部分的 AD FS 部署。 提供建議，在此 AD FS 容量規劃內容，以及提供 AD FS 容量規劃試算表的結果為基礎的假設，每一部同盟伺服器會使用大約 4 GB 的 RAM 的大部分的 AD FS 生產環境環境。  
  
產品小組會使用下列設定，來蒐集適用於同盟伺服器 Proxy 測試的效能與延展性資料：  
  
-   雙四核心 2.24 GHz \(4 核心\)  
  
-   4\-GB RAM  
  
-   Windows Server 2008 R2 Enterprise Edition  
  
-   Gigabit 網路  
  
> [!NOTE]  
> 對於 AD FS 伺服器的容量建議可以變化相當大，視您選擇的硬體和網路設定來指定環境中的規格而定。 基於參考目的，本內容中提供的大小調整指導方針是以稍早所提及之電腦上 80 % 的使用率目標為依據。  
  
## <a name="measure-ad-fs-server-capacity"></a>測量 AD FS 伺服器容量  
一般而言，影響伺服器效能和延展性的硬體元件為 CPU、記憶體、磁碟及網路介面卡。 幸運的是，每個 AD FS 元件需要極少的記憶體和磁碟空間需求。 網路連線是一個顯著的需求。 因此，在同盟伺服器和同盟伺服器 Proxy 上執行的載入測試會集中在兩個用於測量伺服器容量的主要領域上：  
  
-   **每秒的尖峰 AD FS 要求：** 正負號的數字\-在同盟伺服器上每秒處理的要求。 這個度量單位能夠協助您決定可同時登入指定伺服器的使用者數目。 您可以使用這個度量單位搭配 CPU 消耗度量單位，來了解這個度量單位對於效能的影響。  
  
-   **CPU 耗用量：** 衡量 CPU 容量的百分比。 此測量可協助您判斷內送的正負號的數字為基礎的發生的整體 CPU 負載\-每秒的要求。  
  
## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>繼續閱讀以深入了解 AD FS 容量規劃  
您已完成的必要工作，並具有熟悉相關的詞彙與硬體需求之後，您可以使用下列的額外容量規劃內容，協助您判斷所需的 AD FS 伺服器建議的數目您部署：  
  
-   [規劃同盟伺服器容量](Planning-for-Federation-Server-Capacity.md)  
  
-   [規劃同盟伺服器 Proxy 容量](Planning-for-Federation-Server-Proxy-Capacity.md)  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

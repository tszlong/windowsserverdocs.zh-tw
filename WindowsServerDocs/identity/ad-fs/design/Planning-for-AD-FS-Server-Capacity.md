---
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: "AD FS 伺服器容量的計劃"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 484dd08edef85b91e777f8963f175a6172c75430
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-ad-fs-server-capacity"></a>AD FS 伺服器容量的計劃

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

  
> [!NOTE]  
> 本主題提供 content 不會反映實際測試伺服器執行 Windows Server 2012 上執行。 本主題會更新之後，已經執行所需的測試。  
  
容量的 Active Directory 同盟服務計劃 \(AD FS\) 預測使用高峰同盟服務，並計劃的程序，或 scaling\ 接 AD FS 伺服器部署符合這些載入需求。  
  
本章節描述部署指導方針聯盟伺服器和聯盟 proxy 伺服器的角色，並根據實驗室測試 AD FS product 團隊，Microsoft 執行。 本文的目的是可協助您：  
  
-   仔細估計您組織的特定 AD FS 部署，例如 AD FS 伺服器數目的硬體需求。  
  
-   準確投影 sign\ 中要求，成長，計劃的預期的山峰使用量，並確保 AD FS 部署處理預期尖峰使用的功能。  
  
您繼續朗讀此容量計劃 content 之前，我們建議您先完成的工作順序下列兩個表格中所示。 在第一次，我們提供建議的工作，有助於連結提供這個容量規劃討論相關操作。  
  
|建議使用的工作|描述|參考資料|  
|--------------------|---------------|-------------|  
|了解部署 AD FS 聯盟伺服器及聯盟的 proxy 伺服器的需求|檢視重要硬體與軟體需求必要部署聯盟伺服器及聯盟的 proxy 伺服器。|[答附錄審查 AD FS 需求](Appendix-A--Reviewing-AD-FS-Requirements.md)|  
|選取 [AD FS，您將會在組織中部署設定資料庫類型|您可以開始使用容量計劃的資料在本區段中之前，先將判斷 AD FS 設定資料庫類型部署，Windows 內部資料庫 \(WID\) 或結構化查詢語言 \(SQL\) 資料庫。|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md);<br /><br />[AD FS 部署拓撲注意事項](AD-FS-Deployment-Topology-Considerations.md)|  
|判斷拓撲配置，請使用 AD FS 設定資料庫選擇新的類型|一旦您已經使用您的部署中認為 AD FS 設定資料庫的類型，您將需要考慮哪些部署拓撲最接近，您將需要位置聯盟伺服器和聯盟 proxy 伺服器 production 環境中。|[判斷您 AD FS 部署拓撲](Determine-Your-AD-FS-Deployment-Topology.md)|  
|了解金鑰 AD FS 相關容量計劃的條款|審查定義的常見容量計畫使用 AD FS 容量討論計劃的條款。|查看一節[AD FS 容量計劃的條款](Planning-for-AD-FS-Server-Capacity.md#bk_terms)本主題|  
  
之後您已經檢視 content 上表，您現在可以完成下一個表格必要的工作。  
  
|必要條件工作|描述|參考資料|  
|---------------------|---------------|-------------|  
|下載 AD FS 容量計畫縮放試算表|AD FS 容量規劃縮放試算表可協助您判斷聯盟伺服器 AD FS 聯盟伺服器發電廠部署所需的數目。 如何使用這個試算表指示可用的中的下一步的工作如下所提供的連結。|[AD FS 容量計劃試算表](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|  
|收集會需要單一 sign\ 上 \(SSO\) 存取目標 claims\ 感知應用程式和預期的使用高峰相關的存取權的使用者的資料|收集此使用者資料將用於輸入操作 AD FS 容量規劃縮放試算表中所需的值。|[估計聯盟伺服器，您的組織數目](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|  
|AD FS 容量計劃試算表適用於 Windows Server 2016|Windows Server 2016 的更新的規劃工作表|[AD FS Windows Server 2016 容量計劃](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)  
  
## <a name="bk_terms"></a>AD FS 容量計劃的條款  
下表描述這個容量計劃 AD FS 設計指南一節中常使用的重要條款。 AD FS 條款的完整清單，請查看[了解主要 AD FS 概念](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)。  
  
|詞彙|解析度|  
|--------|--------------|  
|使用者人數|估計的使用者，都必須在一段指定時間，通常是澳地區的山峰活動時間提交要求服務數目。|  
|作用中的使用者|大約平均數目系統，但不是一定提交在一段指定時間的要求上的作用中的使用者。|  
|定義的使用者|理論上最大的使用者計數，通常為基礎的使用者有定義帳號，在系統中。|  
|要求秒|數字可能用提交的要求 \（時談論載入 system\ 上）或處理伺服器 \（時談論伺服器 throughput\）在第二個。 這個度量用規劃伺服器的處理器和的記憶體容量。|  
|目標伺服器的回應速度與使用量|繫結的範圍可接受伺服器效能計量成功。 通常，若的回應性低於或使用量超過目標，系統會被視為多載，而是必要的更多的容量。|  
|Windows 內部資料庫 \(WID\)|可使用另一種 SQL Server 中某些 AD FS 部署預設 AD FS 設定資料庫。|  
  
## <a name="configuration-environment-used-during-ad-fs-testing"></a>AD FS 測試期間所使用的設定環境  
本節設定環境的 AD FS product 小組用來執行它測試。 小組效能和延展性測試聯盟 server 的資料收集，使用下列電腦的硬體、軟體和網路設定：  
  
-   雙重 Quad Core 2.27 gigahertz \(GHz\) \(8 cores\)  
  
-   16\ GB 的 RAM  
  
-   Windows Server 2008 R2、企業版  
  
-   Gigabit 網路  
  
> [!NOTE]  
> 16 GB 的 ram 測試期間聯盟伺服器上使用，雖然中等更多記憶體大小，例如為 4 GB 的每個聯盟伺服器可用於大多數 AD FS 部署。 在這個 AD FS 容量計劃網頁結果提供 AD FS 容量規劃試算表和為基礎的每個聯盟伺服器會使用約假設提供建議 4 GB 的 RAM 的大多數 AD FS production 環境。  
  
Product 小組聯盟伺服器 proxy 測試收集效能和延展性資料用於下列設定：  
  
-   雙重 Quad Core 2.24 GHz \(4 cores\)  
  
-   4\ GB 的 RAM  
  
-   Windows Server 2008 R2、企業版  
  
-   Gigabit 網路  
  
> [!NOTE]  
> AD FS 伺服器容量建議可以根據您選擇的特定環境中使用的硬體及網路設定的規格大幅，而有所不同。 點參考資料，以提供本文中的縮放指南根據使用量目標 80%前面所提到的電腦上。  
  
## <a name="measure-ad-fs-server-capacity"></a>AD FS 測量伺服器容量  
通常會影響伺服器的效能與擴充性的硬體元件的 CPU、記憶體、磁碟、和網路介面卡。 幸好，每個元件 AD FS 需要很少的記憶體和磁碟空間需求。 網路連接是那裏的需求。 因此，聯盟伺服器聯盟的 proxy 伺服器上所執行的測試專注在衡量伺服器容量的兩個主要區域：  
  
-   **尖峰秒 AD FS 要求：** sign\ 中要求處理秒聯盟伺服器上的數字。 這個度量單位可協助您判斷多少同時使用者可以登入指定的伺服器。 了解這個度量單位效果效能，您可以搭配 CPU 消耗度量單位使用這個度量單位。  
  
-   **CPU 消耗：**百分比的 cpu 測量容量。 這個度量單位可協助您判斷發生整體 CPU 載入根據秒的傳入 sign\ 中要求數目。  
  
## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>繼續朗讀 AD FS 容量計劃有關更多  
您已經完成的必要條件工作，並具有熟悉相關的條款和硬體需求之後，您可以使用下列其他容量計畫 content 可協助您判斷您的部署所需的 AD FS 伺服器數目建議：  
  
-   [規劃聯盟伺服器容量](Planning-for-Federation-Server-Capacity.md)  
  
-   [規劃區域的聯盟 Proxy 伺服器的容量](Planning-for-Federation-Server-Proxy-Capacity.md)  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

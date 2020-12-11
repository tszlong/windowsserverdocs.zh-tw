---
description: 深入瞭解：規劃 AD FS Server 容量
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: 規劃 AD FS 伺服器容量
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: f396f86ad3ddf410dbb3adf69ab77afdf3d4bbed
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044646"
---
# <a name="planning-for-ad-fs-server-capacity"></a>規劃 AD FS 伺服器容量



> [!NOTE]
> 本主題所提供的內容並不會反映在執行 Windows Server 2012 的伺服器上所執行的實際測試。 一旦執行必要的測試之後，即會更新這個主題。

Active Directory 同盟服務 AD FS 的容量 \( 規劃 \) 是預測同盟服務的尖峰使用期間，以及規劃或擴充 \- AD FS 伺服器部署以符合這些載入需求的程式。

本節說明同盟伺服器和同盟伺服器 proxy 角色的部署指導方針，而且是以 Microsoft AD FS 產品小組所執行的實驗室測試為基礎。 此內容的目的是要協助您：

-   請仔細估計您組織特定 AD FS 部署的硬體需求，例如 AD FS 伺服器數目。

-   精確地投射登入要求的預期尖峰使用量 \- 、規劃成長，並確保您的 AD FS 部署能夠處理該預期的尖峰使用量。

在您繼續閱讀這個容量規劃內容之前，建議您先依照以下兩個表格中所示的順序來完成工作。 在第一個表格中，我們提供建議工作的連結，以協助提供適用於這個容量規劃討論的相關內容。

|建議的工作|描述|參考|
|--------------------|---------------|-------------|
|瞭解部署 AD FS 同盟伺服器和同盟伺服器 proxy 的需求|檢閱部署同盟伺服器和同盟伺服器 Proxy 所需的重要硬體和軟體需求。|[附錄 A：檢閱 AD FS 需求](Appendix-A--Reviewing-AD-FS-Requirements.md)|
|選取您將在組織中部署的 AD FS 設定資料庫類型|在您開始使用本節中的容量規劃資料之前，您必須先決定將部署哪一種 AD FS 設定資料庫類型，可以是 Windows 內部資料庫 \( WID \) 或結構化查詢語言 (SQL) \( SQL \) database。|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md);<p>[AD FS 部署拓撲考量](AD-FS-Deployment-Topology-Considerations.md)|
|決定要和新的 AD FS 設定資料庫選項搭配使用的拓撲配置類型|一旦決定要在部署中使用的 AD FS 設定資料庫類型之後，就需要考量哪一個部署拓撲最符合您需要在實際執行環境內放置同盟伺服器和同盟伺服器 Proxy 的位置。|[決定您的 AD FS 部署拓撲](Determine-Your-AD-FS-Deployment-Topology.md)|
|瞭解重要的 AD FS 相關容量規劃條款|複習 AD FS 容量規劃討論中所使用的一般容量規劃詞彙的定義。|請參閱本主題中標題為 [AD FS 容量規劃詞彙](Planning-for-AD-FS-Server-Capacity.md#bk_terms)的小節。|

在檢閱上表的內容之後，您現在可以完成下表中的必要工作。

|必要的工作|描述|參考|
|---------------------|---------------|-------------|
|下載 AD FS 容量規劃調整大小試算表|AD FS 容量規劃調整大小試算表可協助您判斷 AD FS 同盟伺服器陣列部署所需的同盟伺服器數目。 以下提供的連結中有如何使用這個試算表的指示，可供第二個工作使用。|[AD FS 容量規劃試算表](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|
|收集將需要對 \- \( \) 目標宣告感知應用程式進行單一登入 SSO 存取， \- 以及與此存取相關聯的預期尖峰使用期限之使用者數目的相關資料|您收集的這個使用者資料將用於 AD FS 容量規劃調整大小試算表內所需的輸入值。|[估計適合組織的同盟伺服器數目](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|
|適用于 Windows Server 2016 的 AD FS 容量規劃試算表|更新的 Windows Server 2016 規劃工作表|[AD FS Windows Server 2016 容量規劃](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)

## <a name="ad-fs-capacity-planning-terms"></a><a name="bk_terms"></a>AD FS 容量規劃詞彙
下表描述在 AD FS 設計指南的這個容量規劃一節中經常使用的重要詞彙。 如需 AD FS 詞彙的完整清單，請參閱 [瞭解重要 AD FS 概念](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)。

|詞彙|定義|
|--------|--------------|
|並行使用者|在指定時段內 (通常是尖峰活動期間)，預期會提交要求給服務的使用者估計數目。|
|作用中使用者|在指定時段內，系統上處於作用中狀態但不一定會提交要求之使用者的大約平均數目。|
|定義的使用者|理論上的使用者計數上限，通常會以已在系統中定義帳戶之使用者的數目為依據。|
|每秒要求|當談到系統上的負載時，用戶端所送出的要求數目， \( \) 或伺服器在 \( \) 一秒內討論伺服器輸送量時所處理的要求數目。 規劃伺服器處理器和記憶體容量時會使用此衡量標準。|
|目標伺服器的回應性與使用率|繫結可接受的伺服器效能範圍的成功衡量標準。 通常，如果回應性低於目標或使用率超過目標，便會將系統視為已超載且需更多容量。|
|Windows 內部資料庫 \( WID\)|預設 AD FS 設定資料庫，可用來做為特定 AD FS 部署中 SQL Server 的替代方案。|

## <a name="configuration-environment-used-during-ad-fs-testing"></a>AD FS 測試期間使用的設定環境
本節說明 AD FS 產品小組用來執行測試的設定環境。 小組會使用下列電腦硬體、軟體及網路設定，來蒐集測試同盟伺服器時的效能與延展性資料：

-   雙四核心 2.27 ghz \( \) \( 8 核心\)

-   16 \- GB RAM

-   Windows Server 2008 R2 Enterprise Edition

-   Gigabit 網路

> [!NOTE]
> 雖然在測試期間在同盟伺服器上使用 16 GB 的 RAM，但是在大部分的 AD FS 部署中，都可以使用較適中的記憶體大小，例如每個同盟伺服器 4 GB 的 RAM。 此 AD FS 容量規劃內容中提供的建議，以及 AD FS 容量規劃試算表所提供的結果，都是根據每個同盟伺服器針對大部分 AD FS 生產環境所使用的大約 RAM 4 Gb 的假設。

產品小組會使用下列設定，來蒐集適用於同盟伺服器 Proxy 測試的效能與延展性資料：

-   雙四核心 2.24 GHz \( 四核心\)

-   4 \- GB RAM

-   Windows Server 2008 R2 Enterprise Edition

-   Gigabit 網路

> [!NOTE]
> AD FS 伺服器的容量建議有很大的差異，這取決於您為指定環境中使用的硬體和網路設定所選擇的規格。 基於參考目的，本內容中提供的大小調整指導方針是以稍早所提及之電腦上 80 % 的使用率目標為依據。

## <a name="measure-ad-fs-server-capacity"></a>測量 AD FS 伺服器容量
一般而言，影響伺服器效能和延展性的硬體元件為 CPU、記憶體、磁碟及網路介面卡。 幸運的是，每個 AD FS 元件都需要記憶體和磁碟空間的極少需求。 網路連線是一個顯著的需求。 因此，在同盟伺服器和同盟伺服器 Proxy 上執行的載入測試會集中在兩個用於測量伺服器容量的主要領域上：

-   **每秒尖峰 AD FS 要求數：**\-同盟伺服器上每秒處理的登入要求數目。 這個度量單位能夠協助您決定可同時登入指定伺服器的使用者數目。 您可以使用這個度量單位搭配 CPU 消耗度量單位，來了解這個度量單位對於效能的影響。

-   **CPU 耗用量：** 測量 CPU 容量的百分比。 這項測量可協助您根據每秒傳入的登入要求數目，判斷發生的整體 CPU 負載 \- 。

## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>繼續閱讀以深入了解 AD FS 容量規劃
完成先決條件工作並熟悉相關的條款和硬體需求之後，您可以使用下列額外的容量規劃內容，協助您判斷部署所需的建議 AD FS 伺服器數目：

-   [規劃同盟伺服器容量](Planning-for-Federation-Server-Capacity.md)

-   [規劃同盟伺服器 Proxy 容量](Planning-for-Federation-Server-Proxy-Capacity.md)

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

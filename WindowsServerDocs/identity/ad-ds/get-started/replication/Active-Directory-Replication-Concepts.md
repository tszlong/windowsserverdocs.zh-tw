---
ms.assetid: 4cc9c16c-1928-4dce-a3a8-6229be28eb65
title: "Active Directory 複寫概念"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 45753c048f9bb1cb174daade1d408b88b3686b4b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-replication-concepts"></a>Active Directory 複寫概念

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設計拓撲網站時前, 熟悉一些 Active Directory 複寫概念。  
  
-   [連接物件](#BKMK_1)  
  
-   [KCC](#BKMK_2)  
  
-   [錯誤後移轉功能](#BKMK_3)  
  
-   [子網路](#BKMK_4)  
  
-   [網站](#BKMK_5)  
  
-   [網站連結](#BKMK_6)  
  
-   [網站連結橋接器](#BKMK_7)  
  
-   [網站連結轉移](#BKMK_8)  
  
-   [通用伺服器](#BKMK_9)  
  
-   [快取通用群組成員資格](#BKMK_10)  
  
## <a name="BKMK_1"></a>連接物件  
連接物件是表示目的地網域控制站的來源網域控制站的複寫連接 Active Directory 物件。 網域控制站單一網站的成員，並都會在網站中 Active Directory Domain Services (AD DS) 伺服器物件。 每個伺服器物件有 NTDS 設定物件代表複製網域控制站在網站中的某位子女。  
  
連接是 NTDS 設定物件目的伺服器上的子女。 複製之間兩個網域控制站伺服器物件的其中一個必須連接物件代表輸入的複寫從其他。 所有複寫連接網域控制站都儲存為 NTDS 設定物件下連接物件。 連接物件辨識複寫來源伺服器、包含複寫排程及指定複寫傳輸。  
  
知識一致性檢查程式 (KCC) 會自動建立連接物件，但它們也可以建立以手動方式。 建立者 KCC 連接物件會出現在 Active Directory 網站和服務] 嵌入式管理單元為**<automatically generated>**並視為適當在正常運作的條件。 系統管理員所建立的連接物件手動建立連接物件。 建立時，系統管理員指派的名稱，可連接手動建立的物件。 當您修改**<automatically generated>**連接物件，您將它轉換成系統管理員修改的連接物件與物件 GUID 的形式顯示。 KCC 不會以手動或修改連接物件進行變更。  
  
## <a name="BKMK_2"></a>KCC  
KCC 是所有網域控制站上執行，並產生複寫 Active Directory 樹系拓撲建程序。 KCC 建立不同複寫拓撲根據是否複寫（站台間）網站或之間網站（間）。 KCC 也動態調整拓撲容納新增新的網域控制站在移除現有的網域控制站、移動的網域控制站的網站、變更成本及排程暫時無法使用的網域控制站或發生錯誤。  
  
網站，請在間寫入網域控制站是隨時以雙向更新步調，以減少延遲大的網站以其他快顯連接。 手動，間拓撲是層次的跨樹，這表示一間連接才會出現任何兩個網站的每個 directory 磁碟分割時，且通常不包含快顯連接。 跨樹與 Active Directory 複寫拓撲的相關詳細資訊，會看到 Active Directory 複寫拓撲技術參考 ([https://go.microsoft.com/fwlink/?LinkID=93578](https://go.microsoft.com/fwlink/?LinkID=93578))。  
  
每個網域控制站，KCC 建立複寫路徑建立單向輸入的連接物件定義從其他網域控制站連接。 網域控制站在相同的網站，KCC 建立連接物件自動介入管理。 當您有多個網站時，您設定的網站連結之間的網站，並在每個網站單一 KCC 自動建立之間網站連接也。  
  
### <a name="kcc-improvements-for-windows-server-2008-rodcs"></a>Windows Server 2008 Rodc KCC 改進  

有許多 KCC 改進，以配合 Windows Server 2008 的新提供唯讀網域控制站 (RODC)。 RODC 一般部署案例是分公司。 本案例中最常部署的複寫 Active Directory 拓撲根據少數 bridgehead 伺服器中樞網站的分支」的網域控制站在多個網站複製中樞支點設計。  
  
其中一個優點部署 RODC 在本案例中為單向複寫。 伺服器 bridgehead 就不需要從 RODC，可減少管理和網路使用複製的。  
  
不過，系統挑戰，在舊版 Windows Server 作業系統的中心輻拓撲反白顯示是中樞中新增新的 bridgehead 網域控制站之後, 還有轉散發複寫連接中樞網域控制站利用新中樞網域控制站的分支」的網域控制站之間不自動機制。  
  
Windows Server 2003 網域控制站，您可以重新工作負載平衡所使用的工具，例如 Adlb.exe 從 Windows Server 2003 分支 Office 部署指南 ([https://go.microsoft.com/fwlink/?LinkID=28523](https://go.microsoft.com/fwlink/?LinkID=28523))。  
  
Windows Server 2008 rodc，正常運作的 KCC 提供一些平衡，讓您不需要使用其他工具，例如 Adlb.exe。 預設會讓的新功能。 您可以停用來新增下列設定 RODC 機碼：  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters**  
  
**[隨機 BH Loadbalancing 允許」**  
**1 = 啟用（預設值）0 = 停用**  
  
這些 KCC 改進的運作方式的相關詳細資訊，會看到規劃和部署 Active Directory Domain Services 進行分公司 ([https://go.microsoft.com/fwlink/?LinkId=107114](https://go.microsoft.com/fwlink/?LinkId=107114))。  
  
## <a name="BKMK_3"></a>錯誤後移轉功能  
請確定複寫失敗網路與離線網域控制站路由傳送網站。 調整 AD DS 中進行的變更︰ 複寫拓撲指定的時間間隔可以執行 KCC，例如時新增新的網域控制站的新網站建立與。 KCC 評論複寫狀態的現有的連接，以判斷任何連接會無法運作。 如果因為失敗的網域控制站連接無法運作，KCC 自動組建暫時連接到其他協力（如果有的話）確保複寫進行。 如果所有網域控制站在網站中的無法使用，KCC 會自動建立網域控制站另一網站之間複寫連接。  
  
## <a name="BKMK_4"></a>子網路  
子網路是一組邏輯 IP 位址指派給 TCP/IP 網路的區段。 子網路群組的電腦將在網路上的實體近接辨識的方式。 子網路物件 AD DS 中的找出所使用的網站以地圖電腦的網路位址。  
  
## <a name="BKMK_5"></a>網站  
網站的 Active Directory 物件代表連接高度可靠且更快速的網路的一或多個 TCP/IP 子網路。 網站資訊可讓系統管理員設定 Active Directory 存取和複寫最佳化實體網路的使用量。 物件網站的相關聯的一組子網路，而且森林中的每個網域控制站相關聯的 Active Directory 網站根據其 IP 位址。 網站可以裝載網域控制站的多個網域，並網域可以會以多個網站。  
  
## <a name="BKMK_6"></a>網站連結  
Active Directory 物件代表邏輯路徑 KCC 用來建立複寫 Active Directory 連接的網站連結。 網站連結物件代表一組可以透過指定間傳輸統一成本通訊的網站。  
  
被視為網站連結中所包含的所有網站使用相同的網路類型連接。 網站必須手動連結至其他網站使用的網站連結，以便在某個網站的網域控制站可以從在另一部網站網域控制站複寫 directory 變更。 因為不符網站連結複製期間拍攝網路封包實體網路上的實際路徑，您不需要建立備援網站連結，以改善 Active Directory 複寫效率。  
  
連接兩個網站的網站連結，複寫系統會自動建立特定的網域控制站在每個網站稱為 bridgehead 伺服器之間連接。 Windows Server 2008、網站的所有主機相同的 directory 磁碟分割的網域控制站將為已選取為 bridgehead 伺服器的候選項目。 建立者 KCC 複寫連接隨機閃爍的問題分散在所有的候選項目 bridgehead 伺服器中分享複寫工作負載的網站。 根據預設，隨機的選擇程序發生一次，當連接物件第一次新增至該網站。  
  
## <a name="BKMK_7"></a>網站連結橋接器  
網站連結橋接器為 Active Directory 物件代表一組的網站連結，其網站的所有可以通訊使用一般的傳輸。 網站連結橋接器讓網域控制站不會直接連接透過通訊彼此的連結。 通常網站連結橋接器對應至路由器（或一組路由器）IP 網路。  
  
根據預設，KCC 可以形成轉移透過有共同的某些網站的所有網站連結。 如果已停用這種情形時，每個網站連結表示它自己的不同，且隔離網路。 透過網站連結橋接器表示的網站連結，會被視為單一之前的路徑。 每個橋接器表示隔離的通訊網路流量的環境。  
  
網站連結橋接器的邏輯代表轉移網站之間的實體連接的機制。 網站連結橋接器可 KCC 使用包含的網站的任何的連結組合來判斷連接 directory 磁碟分割中這些網站保存最便宜路徑。 網站連結橋接器不提供的網域控制站的實際連接。 移除網站連結橋接器時，如果複寫組合的網站連結到將會繼續 KCC 移除的連結。  
  
網站連結橋接器時才會網站包含網域控制站裝載的 directory 磁碟分割，也不裝載網域控制站在相鄰網站上，但裝載 directory 磁碟分割網域控制站位於一或多個其他網站森林中。 定義相鄰網站以包含在單一網站連結任何兩個或更多的網站。  
  
網站連結橋接器邏輯之間建立連接兩個網站連結，為兩個中斷連接網站使用暫時之網站間的轉移路徑。 為了間拓撲發電機 (ISTG)，橋接器表示使用暫時網站的實體連接。 橋接器並不代表暫時網站的網域控制站將會提供複寫路徑。 不過，這會是因為暫時網站包含網域控制站裝載複寫 directory 磁碟分割，此時網站連結橋接器不需要。  
  
新增的每個網站連結的費用，建立的結果路徑總和應的成本。 如果暫時網站並未包含網域控制站主控 directory 磁碟分割，並不存在成本較低的連結，就會使用橋接器網站連結。 暫時網站包含網域控制站裝載 directory 磁碟分割，如果有兩個中斷連接的網站會複寫連接到暫時的網域控制站設定，然後使用橋接器。  
  
## <a name="BKMK_8"></a>網站連結轉移  
預設所有網站的連結轉移，或「橋接」。 當橋網站的連結，排程重疊 KCC 建立複寫連接，以判斷網域控制站複寫合作夥伴之間網站的網站不會直接連接網站的連結，但間接連接透過一組常用網站的位置。 這表示，您可以連接任何網站的網站連結組合透過任何其他網站。  
  
一般而言，完全路由網路，您不需要建立任何網站連結橋接器，除非您想要的變更︰ 複寫掌控。 如果完全不路由傳送您的網路，以避免嘗試：鬼影行動複寫應該建立網站連結橋接器。 該傳輸的單一網站連結橋接隱含屬於特定傳輸所有網站連結。 網站連結預設橋接會自動，不 Active Directory 物件代表該橋接器。 **所有網站的連結，ios 都橋接器**設定，請在 IP 和簡易郵件傳輸通訊協定 (SMTP) 間傳輸容器的屬性中找到實作橋接自動網站連結。  
  
> [!NOTE]  
> AD ds; 未來版本中將不支援 SMTP 複寫因此，不建議的網站連結物件建立 SMTP 容器中。  
  
## <a name="BKMK_9"></a>通用伺服器  
通用伺服器是儲存在樹系資訊所有物件的網域控制站，讓應用程式可以搜尋 AD DS，而不需要參考特定網域控制站的市集要求的資料。 所有網域控制站，例如通用伺服器會儲存完整的寫入架構與設定 directory 磁碟分割複本完整、寫入網域它裝載的網域 directory 磁碟分割複本。 此外，通用伺服器會儲存森林中的每個其他的網域部分、唯讀複本。 部分、唯讀網域複本包含每個網域，但僅限子集屬性（最常使用的搜尋物件的屬性）中的物件。  
  
## <a name="BKMK_10"></a>快取通用群組成員資格  
快取通用群組成員資格可網域控制站使用者快取通用群組成員資格資訊。 您可以讓網域控制站的 Windows Server 2008 快取通用群組成員資格使用來執行 Active Directory 網站與服務] 嵌入式管理單元。  
  
讓通用登入，就不需要在每個網站使用網路頻寬最小化，因為網域控制站不需要複製所有位於森林中的物件網域中的通用伺服器。 它也會降低登入時間因為驗證網域控制站不一定需要存取通用取得通用群組成員資格資訊。 如需有關使用通用群組成員資格快取的時機，請查看[規劃全球 Catalog 伺服器位置](../../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)。  
  



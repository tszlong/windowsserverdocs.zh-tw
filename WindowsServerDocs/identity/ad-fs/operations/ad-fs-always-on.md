---
title: 使用 AlwaysOn 可用性群組設定 AD FS 部署
description: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/20/2020
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2ea32943f8b46718b90c30024da883c1a35f3888
ms.sourcegitcommit: b649047f161cb605df084f18b573f796a584753b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/17/2020
ms.locfileid: "76162742"
---
# <a name="setting-up-an-ad-fs-deployment-with-alwayson-availability-groups"></a>使用 AlwaysOn 可用性群組設定 AD FS 部署
高可用性地理分佈拓撲提供：
* 消除單一失敗點：使用容錯移轉功能時，您可以達到高可用性的 ADFS 基礎結構，即使地球中的其中一個資料中心停止運作也一樣。
* 改善的效能：您可以使用建議的部署來提供高效能的 ADFS 基礎結構

AD FS 可以針對高可用性地理位置分散案例進行設定。
下列指南將逐步介紹 SQL Always on 可用性群組的 AD FS，並提供部署考慮和指引。

## <a name="overview---alwayson-availability-groups"></a>總覽-AlwaysOn 可用性群組

如需 AlwaysOn 可用性群組的詳細資訊，請參閱[AlwaysOn 可用性群組的總覽（SQL Server）](https://technet.microsoft.com/library/ff877884.aspx)

從 AD FS SQL Server 服務器陣列的節點觀點來看，AlwaysOn 可用性群組會將單一 SQL Server 實例取代為原則/成品資料庫。  「可用性群組接聽程式」是用戶端（AD FS Security Token Service）用來連接到 SQL 的功能。
下圖顯示具有 AlwaysOn 可用性群組的 AD FS SQL Server 服務器陣列。

![使用 SQL 的伺服器陣列](media/ad-fs-always-on/SQLoverview.png)

Always On 可用性群組（AG）是一或多個一起故障切換的使用者資料庫。 可用性群組是由主要可用性複本和一到四個次要複本所組成，透過 SQL Server 記錄式資料移動來維護，而不需要共用儲存體。 每個複本都是由 WSFC 的不同節點上的 SQL Server 實例所主控。 可用性群組和對應的虛擬網路名稱會註冊為 WSFC 叢集中的資源。

主要複本節點上的可用性群組接聽程式會回應連接到虛擬網路名稱的內送用戶端要求，並根據連接字串中的屬性，將每個要求重新導向至適當的 SQL Server 實例。
發生容錯移轉時，會利用 WSFC，將另一個 SQL Server 實例上的次要複本重新設定為可用性群組的主要複本，而不會將共用實體資源的擁有權轉移至另一個節點。 然後可用性群組的虛擬網路名稱資源會轉移至該執行個體。
在任何指定的時間，只有一個 SQL Server 實例可以裝載可用性群組之資料庫的主要複本，所有相關聯的次要複本都必須位於個別的實例上，而且每個實例必須位於個別的實體節點上。

> [!NOTE] 
> 如果機器是在 Azure 上執行，請設定 Azure 虛擬機器，讓接聽程式設定能夠與 AlwaysOn 可用性群組進行通訊。 如需詳細資訊，請[虛擬機器： SQL Always On](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener)接聽程式。

如需 AlwaysOn 可用性群組的其他總覽，請參閱[Always On 可用性群組（SQL Server）的總覽](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server?view=sql-server-ver15)。

> [!NOTE] 
> 如果組織需要在多個資料中心之間進行容錯移轉，建議您在每個資料中心內建立成品資料庫，並啟用背景快取，以減少要求處理期間的延遲。 請依照指示來進行，[微調 SQL 並減少延遲](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/adfs-sql-latency)。

## <a name="deployment-guidance"></a>部署指引

1. <b>針對 AD FS 部署的目標，請考慮正確的資料庫。</b>AD FS 使用資料庫來儲存設定，以及在某些情況下，與同盟服務相關的交易資料。 您可以使用 AD FS 軟體來選取內建 Windows 內部資料庫（WID）或 Microsoft SQL Server 2008 或更新版本，以將資料儲存在 federation service 中。
下表說明 WID 與 SQL database 之間支援的功能差異。


| 分類      | 功能       | 受 WID 支援  | 支援 SQL |
| ------------------ |:-------------:| :---:|:---: |
| AD FS 功能     | 同盟伺服器陣列部署 | [是]  | [是] |
| AD FS 功能     | SAML 成品解析。 注意：這不是 SAML 應用程式的常見情況     |   無 | 無  |
| AD FS 功能 | SAML/WS-同盟權杖重新執行偵測。 注意：只有在 AD FS 收到來自外部 Idp 的權杖時才需要。 如果 AD FS 不是 IDP，就不需要這麼做。      |    無  | [是] |
| 資料庫功能     |   使用提取複寫的基本資料庫冗余，其中一或多部裝載資料庫的唯讀複本的伺服器要求變更資料庫的讀取/寫入複本    |   無 | 無  |
| 資料庫功能 | 使用高可用性解決方案的資料庫冗余，例如叢集或鏡像（在資料庫層級）      |    無  | [是] |

如果您是具有超過100個信任關係的大型組織，而且需要為他們的內部使用者和外部使用者提供同盟應用程式或服務的單一登入存取權，則建議使用 SQL 選項。

如果您是具有100或較少已設定之信任關係的組織，WID 會提供資料和同盟服務的冗余（其中每一部同盟伺服器會將變更複寫到相同伺服器陣列中的其他同盟伺服器）。 WID 不支援權杖重新執行偵測或成品解析，而且限制為30部同盟伺服器。
如需規劃部署的詳細資訊，請造訪[這裡](https://docs.microsoft.com/windows-server/identity/ad-fs/design/planning-your-deployment)。

## <a name="sql-server-high-availability-solutions"></a>SQL Server 高可用性解決方案
如果您使用 SQL Server 作為 AD FS 設定資料庫，您可以使用 SQL Server 複寫為您的 AD FS 伺服器陣列設定異地冗余。 異地複寫會在兩個地理位置較遠的網站之間複寫資料，讓應用程式可以從某個網站切換至另一個網站。 如此一來，萬一某個網站失敗，您仍然可以在第二個網站上使用所有設定資料。 
如果 SQL 是適合您部署目標的資料庫，請繼續進行此部署指南。

本指南將逐步解說下列各項
* Deploy AD FS
* 設定 AD FS 使用 AlwaysOn 可用性群組
* 安裝容錯移轉叢集角色
* 執行叢集驗證測試
* 啟用 Always On 可用性群組
* 備份 AD FS 資料庫
* 建立 AlwaysOn 可用性群組
* 在第二個節點上新增資料庫
* 將可用性複本加入可用性群組
* 更新 SQL 連接字串

## <a name="deploy-ad-fs"></a>Deploy AD FS

> [!NOTE] 
> 如果電腦是在 Azure 上執行，則必須以特定方式設定虛擬機器，讓接聽程式能夠與 Always On 的可用性群組進行通訊。 如需設定的相關資訊，請參閱針對[Azure 上的可用性群組設定負載平衡器 SQL Server vm](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener)


此部署指南會顯示兩個節點伺服器陣列，其中包含兩個 SQL 伺服器做為範例。
若要部署 AD FS 請遵循下列初始連結來安裝 AD FS 角色服務。 若要設定 AoA 群組，該角色將會有額外的步驟。
-   [將電腦加入網域](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/join-a-computer-to-a-domain)
-   [註冊 AD FS 的 SSL 憑證](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/enroll-an-ssl-certificate-for-ad-fs)
-   [安裝 AD FS 角色服務](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/install-the-ad-fs-role-service)


## <a name="configuring-ad-fs-to-use-an-alwayson-availability-group"></a>設定 AD FS 以使用 AlwaysOn 可用性群組

設定具有 AlwaysOn 可用性群組的 AD FS 伺服器陣列時，需要稍微修改 AD FS 部署程式。 請確定每個伺服器實例都執行相同版本的 SQL。 若要查看 Always On 可用性群組的必要條件、限制和建議的完整清單，請閱讀[這裡](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability?view=sql-server-2017#PrerequisitesForDbs)。

1.  您必須先建立要備份的資料庫，才能設定 AlwaysOn 可用性群組。  AD FS 會建立其資料庫，做為新 AD FS SQL Server 服務器陣列的第一個 federation service 節點的安裝和初始設定的一部分。  使用 SQL server 指定現有伺服器陣列的資料庫主機名稱。 在 AD FS 設定的過程中，您必須指定 SQL 連接字串，因此必須將第一個 AD FS 陣列設定為直接連接到 SQL 實例（這只是暫時性的）。 如需有關設定 AD FS 伺服器陣列的特定指引，包括使用 SQL server 連接字串設定 AD FS 伺服器陣列節點，請參閱[設定同盟伺服器](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/configure-a-federation-server)。

![指定伺服器陣列](media/ad-fs-always-on/deploymentSpecifyFarm.png)

2.  使用 SSMS 驗證與資料庫的連線，然後連接到目標資料庫主機名稱。 如果將另一個節點加入至同盟伺服器陣列，請連接到目標資料庫。
3.  指定 AD FS 伺服器陣列的 SSL 憑證。

![指定 ssl 憑證](media/ad-fs-always-on/deploymentSpecifySSL.png)

4.  將伺服器陣列連接至服務帳戶或 gMSA。

![指定服務帳戶](media/ad-fs-always-on/deploymentSpecifyServiceAccount.png)

5.  完成 AD FS 伺服器陣列設定和安裝。

> [!NOTE] 
> SQL Server 必須在網域帳戶下執行，才能安裝 Always On 可用性群組。 根據預設，它會以本機系統的形式執行。

## <a name="install-the-failover-clustering-role"></a>安裝容錯移轉叢集角色
Windows Server 容錯移轉叢集角色提供有關 Windows Server 容錯移轉叢集的詳細資訊，
1.  啟動 [伺服器管理員]。
2.  在 [管理] 功能表上，選取 [新增角色及功能]。
3.  在 [開始之前] 頁面上，選取 [下一步]。
4.  在 [選取安裝類型] 頁面上，選取 [角色型或功能型安裝]，然後選取 [下一步]。
5.  在 [選取目的地伺服器] 頁面上，選取您要安裝功能的 SQL server，然後選取 [下一步]。

![目的地伺服器](media/ad-fs-always-on/clusteringDestinationServer.png)

6.  在 [選取伺服器角色] 頁面上，選取 [下一步]。
7.  在 [選取功能] 頁面上，選取 [容錯移轉叢集] 核取方塊。

![選取群集功能](media/ad-fs-always-on/clusteringFeature.png)

8.  在 [確認安裝選項] 頁面上，選取 [安裝]。
容錯移轉叢集功能不需要重新啟動伺服器。
9.  當安裝完成時，請選取 [關閉]。
10. 在想要新增為容錯移轉叢集節點的每部伺服器上重複此程序。

## <a name="run-cluster-validation-tests"></a>執行叢集驗證測試
1.  在已從遠端伺服器管理工具安裝了容錯移轉叢集管理工具的電腦上，或是在安裝了容錯移轉叢集功能的伺服器上，啟動 [容錯移轉叢集管理員]。 若要在伺服器上執行此動作，請啟動伺服器管理員，然後在 [工具] 功能表上，選取 [容錯移轉叢集管理員]。
2.  在 [容錯移轉叢集管理員] 窗格的 [管理] 底下，選取 [驗證設定]。
3.  在 [開始之前] 頁面上，選取 [下一步]。
4.  在 [選取伺服器或叢集] 頁面的 [輸入名稱] 方塊中，輸入您計畫新增為容錯移轉叢集節點之伺服器的 NetBIOS 名稱或完整功能變數名稱，然後選取 [新增]。 對您要新增的每部伺服器重複此步驟。 若要同時新增多部伺服器，請以逗號或分號分隔名稱。 例如，以 server1.contoso.com,  server2.contoso.com 的格式輸入名稱。 完成後，選取 [下一步]。

![選取伺服器圖片](media/ad-fs-always-on/clusterValidationServers.png)

5. 在 [測試選項] 頁面上，選取 [執行所有測試（建議）]，然後選取 [下一步]。
6. 在 [確認] 頁面上，選取 [下一步]。
[驗證中] 頁面會顯示執行測試的狀態。
7. 在 [摘要] 頁面上，執行下列其中一項：
- 如果結果指出測試已順利完成，且設定適用于叢集，而您想要立即建立叢集，請確定已選取 [立即使用已驗證的節點建立叢集] 核取方塊，然後選取結束. 然後，繼續進行[建立容錯移轉](https://docs.microsoft.com/windows-server/failover-clustering/create-failover-cluster#create-the-failover-cluster)叢集程式的步驟4。

![驗證設定圖片](media/ad-fs-always-on/clusterValidationResults.png)

-   如果結果指出發生警告或失敗，請選取 [View Report] 來查看詳細資料，並判斷哪些問題必須更正。 請注意，特定驗證測試的警告會指示可支援此部分的容錯移轉叢集，但可能不符合建議的最佳做法。

> [!NOTE]
> 如果您收到「驗證儲存空間持續保留」測試的警告，請參閱部落格文章 [Windows 容錯移轉叢集驗證警告指示您的磁碟不支援持續保留儲存空間](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) ，以了解詳細資訊。
> 如需硬體驗證測試的詳細資訊，請參閱 [Validate Hardware for a Failover Cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11))。

## <a name="create-the-failover-cluster"></a>建立容錯移轉叢集

若要完成此步驟，請確定您用來登入的使用者帳戶符合本主題 [確認先決條件](https://docs.microsoft.com/windows-server/failover-clustering/create-failover-cluster#verify-the-prerequisites) 一節中所列的需求。
1.  啟動 [伺服器管理員]。
2.  在 [工具] 功能表上，選取 [容錯移轉叢集管理員]。
3.  在 [容錯移轉叢集管理員] 窗格的 [管理] 底下，選取 [建立叢集]。
就會開啟 [建立叢集精靈]。
4.  在 [開始之前] 頁面上，選取 [下一步]。
5.  如果出現 [選取伺服器] 頁面，請在 [輸入名稱] 方塊中，輸入您計畫新增為容錯移轉叢集節點之伺服器的 NetBIOS 名稱或完整功能變數名稱，然後選取 [新增]。 對您要新增的每部伺服器重複此步驟。 若要同時新增多部伺服器，請以逗號或分號分隔名稱。 例如，以 server1.contoso.com; server2.contoso.com 的格式輸入名稱。 完成後，選取 [下一步]。

![建立叢集並選取伺服器](media/ad-fs-always-on/createClusterServers.png)

> [!NOTE]
> 如果您選擇在設定[驗證](https://docs.microsoft.com/windows-server/failover-clustering/create-failover-cluster#validate-the-configuration)程式中執行驗證之後立即建立叢集，您將不會看到 [選取伺服器] 頁面。 已驗證的節點會自動新增至 [建立叢集精靈]，讓您不需要重新輸入。

6.  如果您之前略過驗證，就會顯示 [驗證警告] 頁面。 我們強烈建議您執行叢集驗證。 Microsoft 只支援通過所有驗證測試的叢集。 若要執行驗證測試，請選取 [是]，然後選取 [下一步]。 依照[驗證](https://docs.microsoft.com/windows-server/failover-clustering/create-failover-cluster#validate-the-configuration)設定中所述，完成 [驗證設定] Wizard。
7.  在 [管理叢集的存取點] 頁面上，執行下列動作：
-   在 [叢集名稱] 方塊中，輸入您要用來管理叢集的名稱。 執行這個動作之前，請檢閱下列資訊：
 -  在叢集建立期間，會將此名稱註冊為 AD DS 中的叢集電腦物件（也稱為叢集名稱物件或 CNO）。 如果您指定叢集的 NetBIOS 名稱，就會在叢集節點電腦物件所在的相同位置建立 CNO。 這可以是預設的電腦容器或 OU。
 -  若要為 CNO 指定不同的位置，您可以在 [叢集名稱] 方塊中輸入 OU 的辨別名稱。 例如： CN = ClusterName，OU = 叢集，DC = Contoso，DC = com。
 -  如果網域系統管理員已在不同於叢集節點所在的其他 OU 中預先設置 CNO，請指定網域系統管理員提供的辨別名稱。
- 如果伺服器沒有設定使用 DHCP 的網路介面卡，您必須為容錯移轉叢集設定一或多個靜態 IP 位址。 選取您想要用於叢集管理的每個網路旁邊的核取方塊。 選取所選網路旁邊的 [位址] 欄位，然後輸入您要指派給叢集的 IP 位址。 此 IP 位址 (或多個位址) 會與網域名稱系統 (DNS) 的叢集名稱相關聯。
- 完成後，選取 [下一步]。

8.  檢視 [確認] 頁面上的設定。 預設會選取 [新增適合的儲存裝置到叢集] 核取方塊。 如果您想要執行下列其中一項，請取消選取此核取方塊：
-   您想要稍後再設定存放裝置。
-   您計畫透過 [容錯移轉叢集管理員] 或透過容錯移轉叢集 Windows PowerShell Cmdlet 來建立叢集儲存空間，且尚未在檔案和存放服務中建立儲存空間。 如需詳細資訊，請參閱 [Deploy Clustered Storage Spaces](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11))。
9.  選取 [下一步] 以建立容錯移轉叢集。
10. 在 [摘要] 頁面上，確認已順利建立容錯移轉叢集。 如果出現任何警告或錯誤，請查看摘要輸出或選取 [查看報表]，以查看完整的報表。 選取 [完成]。
11. 若要確認已建立叢集，請確認瀏覽樹狀目錄中 [容錯移轉叢集管理員] 底下有列出叢集名稱。 您可以展開叢集名稱，然後選取 [節點]、[儲存體] 或 [網路] 底下的專案，以查看相關聯的資源。
請注意它可能需要一些時間以便成功複寫 DNS 中的叢集名稱。 成功註冊 DNS 及複寫之後，如果您選取伺服器管理員中的 [所有伺服器]，叢集名稱應該會列為管理性狀態為 [線上] 的伺服器。

![建立叢集完成](media/ad-fs-always-on/createClusterComplete.png)

## <a name="enable-always-on-availability-groups-with-sql-server-configuration-manager"></a>使用 SQL Server 組態管理員啟用 Always on 可用性群組

1.  連接到 Windows Server 容錯移轉叢集（WSFC）節點，其裝載您要啟用 Always On 可用性群組的 SQL Server 實例。
2.  在 [開始] 功能表上，依序指向 [所有程式]、[Microsoft SQL Server]、[組態工具]，然後按一下 [SQL Server 組態管理員]。
3.  在 SQL Server 組態管理員中，按一下 [SQL Server 服務]，以滑鼠右鍵按一下 [SQL Server （<instance name>）]，其中 <instance name> 是您要啟用 Always On 可用性群組的本機伺服器實例名稱，然後按一下 [屬性]。
4.  選取 [Always On 高可用性] 索引標籤。
5.  確認 [Windows  容錯移轉叢集名稱] 欄位包含本機容錯移轉叢集的名稱。 如果此欄位為空白，則這個伺服器實例目前不支援 Always On 可用性群組。 可能是本機電腦不是叢集節點、WSFC 叢集已關閉，或此版本的 SQL Server 不支援 Always On 可用性群組。
6.  選取 [啟用 Always On 可用性群組] 核取方塊，然後按一下 [確定]。
SQL Server 組態管理員會儲存您的變更。 然後您必須手動重新啟動 SQL Server 服務。 這讓您可以選擇最適合您業務需求的重新啟動時間。 當 SQL Server 服務重新開機時，Always On 將會啟用，而且 IsHadrEnabled 伺服器屬性會設定為1。

![啟用 AoA](media/ad-fs-always-on/enableAoAGroup.png)

## <a name="back-up-ad-fs-databases"></a>備份 AD FS 資料庫
使用完整的交易記錄備份 AD FS 設定和成品資料庫。 將備份放在所選的目的地。
備份 ADFS 成品和設定資料庫。
- > 備份的工作 > 完整 > 新增至備份檔案 > [確定] 以建立

![備份伺服器](media/ad-fs-always-on/backUpADFS.png)

## <a name="create-new-availability-group"></a>建立新的可用性群組

1.  在 [物件總管] 中，連接到裝載主要複本的伺服器執行個體。
2.  展開 [Always On 高可用性] 節點和 [可用性群組] 節點。
3.  若要啟動 [新增可用性群組精靈]，請選取 [新增可用性群組精靈] 命令。
4.  當您初次執行此精靈時，將會出現 [簡介] 頁面。 如果將來要略過此頁面，您可以按一下 [不要再顯示此頁面]。 閱讀這個頁面之後，請按 [下一步]。
5.  在 [指定可用性群組選項] 頁面上，于 [可用性組名] 欄位中輸入新的可用性組名。 此名稱必須是在叢集和整個網域中唯一的有效 SQL Server 識別碼。 可用性群組名稱的最大長度為 128 個字元。 e
6.  接下來，指定叢集類型。 可能的叢集類型取決於 SQL Server 版本和作業系統。 選擇 [WSFC]、[外部] 或 [無]。 如需詳細資訊，請參閱[指定可用性組名](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/specify-availability-group-name-page?view=sql-server-ver15)頁面

![名稱 AoA 群組和叢集](media/ad-fs-always-on/createAoAName.png)

7.  在 [選取資料庫] 頁面上，方格會列出連接之伺服器執行個體上有資格變成「可用性資料庫」(Availability Database) 的使用者資料庫。 請選取一個或多個列出的資料庫，以便參與新的可用性群組。 這些資料庫一開始將會成為初始「主要資料庫」(Primary Database)。
針對每個列出的資料庫，[大小] 資料行會顯示資料庫大小 (如果已知的話)。 [狀態] 欄指出給定的資料庫是否符合可用性資料庫的[必要條件](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability?view=sql-server-ver15)。 如果不符合必要條件，簡短狀態描述會指出資料庫不合格的原因。例如，資料庫沒有使用完整復原模式。 如需詳細資訊，請按一下狀態描述。
如果您變更資料庫讓它符合資格，請按一下 [重新整理] 更新資料庫方格。
如果資料庫含有資料庫主要金鑰，請在 [密碼] 資料行輸入資料庫主要金鑰的密碼。

![選取要 AoA 的資料庫](media/ad-fs-always-on/createAoASelectDb.png)

8. 在 [指定複本] 頁面上，為新的可用性群組指定並設定一個或多個複本。 此頁面包含四個索引標籤。 下表將介紹這些索引標籤。 如需詳細資訊，請參閱[指定複本頁面（新增可用性群組 wizard：加入複本嚮導）](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard?view=sql-server-ver15)主題。

| Tab      | 簡短描述       |
| ------------------ |:-------------:|
| 複本     | 使用此索引標籤可指定將裝載次要複本的每個 SQL Server 實例。 請注意，您目前所連接的伺服器執行個體必須裝載主要複本。 |
| 端點     | 使用此索引標籤可驗證任何現有的資料庫鏡像端點，此外，如果在其服務帳戶使用 Windows 驗證的伺服器執行個體上缺少此端點，則可自動建立該端點。|
| 備份喜好設定 | 使用此索引標籤可指定整個可用性群組的備份喜好設定，以及個別可用性複本的備份優先權。      |
| Listener     | 使用此索引標籤可建立可用性群組接聽程式。 根據預設，精靈不會建立接聽程式。      |

![指定複本詳細資料](media/ad-fs-always-on/createAoAchooseReplica.png)

9. 在 [選取初始資料同步處理] 頁面上，選擇您要如何建立新的次要資料庫並將它聯結至可用性群組。 選擇下列其中一個選項：
-   自動植入
 - SQL Server 會自動為群組中的每個資料庫建立次要複本。 自動植入要求參與群組之每個 SQL Server 執行個體上的資料和記錄檔案路徑都必須相同。 適用于 SQL Server 2016 （13. x）和更新版本。 請參閱[自動初始化 Always On 可用性群組](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group?view=sql-server-ver15)。
- 完整資料庫和記錄備份
 - 如果您的環境符合自動啟動初始資料同步處理的需求，請選取此選項（如需詳細資訊，請參閱[本主題稍早的必要條件、限制和建議）](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio?view=sql-server-ver15#Prerequisites)。
如果您選取 [完整]，在建立可用性群組之後，精靈會將每個主要資料庫及其交易記錄備份至網路共用，並在裝載次要複本的每個伺服器執行個體上還原這些備份。 然後精靈會將每個次要資料庫聯結至可用性群組。
在 [指定所有複本可存取的共用網路位置:] 欄位中，指定裝載複本的所有伺服器執行個體都有讀寫存取的備份共用。 如需詳細資訊，請參閱本主題前面的＜必要條件＞。 在驗證步驟中，此精靈會執行測試以確定所提供的網路位置有效，測試會在主要複本上建立名為 "BackupLocDb_" 的資料庫，後面接著 GUID，並備份到所提供的網路位置，然後在次要複本上將它還原。 您可以放心地將此資料庫隨其備份歷程記錄和備份檔案一同刪除 (如果精靈無法將其刪除)。
- 僅加入
 - 如果您已經在將裝載次要複本的伺服器執行個體上手動備妥次要資料庫，就可以選取此選項。 然後精靈會將現有的次要資料庫聯結至可用性群組。
- 略過初始資料同步處理
 - 如果要使用您自己的主要資料庫的資料庫和記錄備份，請選取此選項。 如需詳細資訊，請參閱[在 Always On 次要資料庫上啟動資料移動（SQL Server）](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server?view=sql-server-ver15)。

![選擇資料同步選項](media/ad-fs-always-on/createAoADataSync.png)

9.  [驗證] 頁面會驗證您在此精靈中指定的值是否符合 [新增可用性群組精靈] 的需求。 若要進行變更，您可以按 [上一步] 返回先前的精靈頁面，以變更一個或多個值。 然後按 [下一步] 返回 [驗證] 頁面，再按一下 [重新執行驗證]。

10. 在 [摘要] 頁面上，檢閱您為新的可用性群組的選擇。 若要進行變更，請按 [上一步] 返回相關頁面。 進行變更之後，請按 [下一步] 返回 [摘要] 頁面。

> [!NOTE] 
> 當要裝載新可用性複本之伺服器實例的 SQL Server 服務帳戶尚未以登入形式存在時，新的可用性群組 Wizard 必須建立登入。 在 [摘要] 頁面上，精靈會顯示要建立之登入的資訊。 如果您按一下 [完成]，精靈就會為 SQL Server 服務帳戶建立此登入，並且授與登入 CONNECT 權限。
> 如果您對所做的選擇感到滿意時，可以選擇按一下 [指令碼]，建立精靈將執行之步驟的指令碼。 然後，若要建立及設定新的可用性群組，請按一下 [完成]。

11. [進度] 頁面會顯示建立可用性群組之步驟的進度 (設定端點、建立可用性群組，並將次要複本加入群組中)。
12. 當這些步驟完成時，[結果] 頁面會顯示每個步驟的結果。 如果所有這些步驟都成功，表示新的可用性群組已完成設定。 如果任何步驟導致錯誤，您可能需要手動完成組態，或者針對失敗的步驟使用精靈。 如需有關給定錯誤原因的詳細資訊，請按一下 [結果] 資料行中相關聯的 [錯誤] 連結。
當精靈完成時，按一下 [關閉] 以結束。

![驗證完成](media/ad-fs-always-on/createAoAValidation.png)

## <a name="add-databases-on-secondary-node"></a>在次要節點上新增資料庫

1.  使用所建立的備份檔案，透過次要節點上的 UI 還原成品資料庫。
![透過 UI](media/ad-fs-always-on/restoreDB.png) 還原

2. 以非復原狀態還原資料庫。
使用非復原的 ![還原](media/ad-fs-always-on/restoreNonRecovery.png)

3. 重複執行還原設定資料庫的程式。

## <a name="join-availability-replica-to-an-availability-group"></a>將可用性複本加入可用性群組

1.  在 [物件總管] 中，連接到裝載次要複本的伺服器執行個體，然後按一下伺服器名稱以展開伺服器樹狀目錄。
2.  展開 [Always On 高可用性] 節點和 [可用性群組] 節點。
3.  選取所連接之次要複本的可用性群組。
4.  以滑鼠右鍵按一下次要複本，然後按一下 [加入可用性群組]。
5.  這會開啟 [將複本加入至可用性群組] 對話方塊。
6.  若要將次要複本聯結至可用性群組，請按一下 [確定]。

![聯結次要複本](media/ad-fs-always-on/jointoAoA.png)

## <a name="update-the-sql-connection-string"></a>更新 SQL 連接字串
最後，使用 PowerShell 來編輯 AD FS 屬性，以更新 SQL 連接字串，以使用 AlwaysOn 可用性群組接聽程式的 DNS 位址。
在每個節點上執行設定資料庫變更，並在所有 ADFS 節點上重新開機 ADFS 服務。 [初始目錄] 值會根據伺服器陣列版本而變更。

```
PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService
PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”
PS:\>$temp.put()
PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”
```
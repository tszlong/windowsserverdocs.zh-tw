---
title: 新增 RD 連線代理人伺服器以設定 RDS 中的高可用性
description: 了解如何將 RD 連線代理人新增至 RDS 部署以達到高可用性。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: dc6a9fa0d6834f63c9935518e4b2c26320a04082
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80852961"
---
# <a name="add-the-rd-connection-broker-server-to-the-deployment-and-configure-high-availability"></a>將 RD 連線代理人伺服器加入部署並設定高可用性

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

您可以部署遠端桌面連線代理人 (RD 連線代理人) 叢集，以提升遠端桌面服務基礎結構的可用性和延展性。 

## <a name="pre-requisites"></a>必要條件

設定伺服器以作為第二個 RD 連線代理人 - 這可以是實體伺服器或 VM。

設定連線代理人的資料庫。 您可以使用 [Azure SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-get-started/#create-a-new-aure-sql-database) 執行個體或您本機環境中的 SQL Server。 以下的討論將使用 Azure SQL，但這些步驟仍適用於 SQL Server。 您必須尋找資料庫的連接字串，並確定您有正確的 ODBC 驅動程式。

## <a name="step-1-configure-the-database-for-the-connection-broker"></a>步驟 1：設定連線代理人的資料庫

1. 尋找您所建立之資料庫的連接字串 - 您需要用它來識別您所需的 ODBC 驅動程式版本，且後續在設定連線代理人本身 (步驟 3) 時也需用到此字串，因此請此將字串儲存在方便參考之處。 以下說明如何尋找 Azure SQL 的連接字串：  
    1. 在 Azure 入口網站中，按一下 [瀏覽] > [資源群組]  ，然後按一下部署的資源群組。   
    2. 選取您剛建立的 SQL 資料庫 (例如 CB-DB1)。   
    3. 按一下 [設定]   > [內容]   > [顯示資料庫連接字串]  。   
    4. 複製 **ODBC (包含 Node.js)** 的連接字串，顯示如下：   
      
        ```
        Driver={SQL Server Native Client 13.0};Server=tcp:cb-sqls1.database.windows.net,1433;Database=CB-DB1;Uid=sqladmin@contoso;Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
        ```
  
    5. 將 "your_password_here" 取代為實際的密碼。 連線至資料庫時，您將會用到這整個字串與內含的密碼。 
2. 在新的連線代理人上安裝 ODBC 驅動程式： 
   1. 如果您使用 VM 作為連線代理人，請建立第一個 RD 連線代理人的公用 IP 位址。 (只有在 RDMS 虛擬機器還沒有公用 IP 位址以允許 RDP 連線時，才需要這麼做。)
       1. 在 Azure 入口網站中，按一下 [瀏覽]   > [資源群組]  、按一下用於部署的資源群組，然後按一下第一個 RD 連線代理人虛擬機器 (例如 Contoso-Cb1)。
       2. 按一下 [設定 > 網路介面]  ，然後按一下對應的網路介面。
       3. 按一下 [設定] > [IP 位址]  。
       4. 針對 [公用 IP 位址]  選取 [已啟用]  ，然後按一下 [IP 位址]  。
       5. 如果您目前有想要使用的公用 IP 位址，請從清單中加以選取。 否則，請按一下 [新建]  、輸入名稱，然後依序按一下 [確定]  和 [儲存]  。
   2. 連線至第一個 RD 連線代理人：
       1. 在 Azure 入口網站中，按一下 [瀏覽]   > [資源群組]  、按一下用於部署的資源群組，然後按一下第一個 RD 連線代理人虛擬機器 (例如 Contoso-Cb1)。
       2. 按一下 [連線 > 開啟]  以開啟遠端桌面用戶端。
       3. 在用戶端按一下 [連線]  ，然後按一下 [使用其他使用者帳戶]  。 輸入網域系統管理員帳戶的使用者名稱和密碼。
       4. 在出現憑證相關警告時，按一下 [是]  。
   3. 下載與 ODBC 連接字串中的版本相符的[適用於 SQL Server 的 ODBC 驅動程式](https://www.microsoft.com/download/confirmation.aspx?id=50420)。 針對上方的範例字串，我們需要安裝第 13 版的 ODBC 驅動程式。
   4. 將 sqlincli.msi 檔案複製到第一個 RD 連線代理人伺服器。   
   5. 開啟 sqlincli.msi 檔案，並安裝原生用戶端。  
   6. 對每個額外的 RD 連線代理人 (例如 Contoso-Cb2) 重複步驟 1-5。
   7. 在將執行連線代理人的每個伺服器上安裝 ODBC 驅動程式。

## <a name="step-2-configure-load-balancing-on-the-rd-connection-brokers"></a>步驟 2：設定 RD 連線代理人的負載平衡 

如果您使用 Azure 基礎結構，您可以建立 [Azure Load Balancer](#create-a-load-balancer)；或者，您可以設定 [DNS 循環配置資源](#configure-dns-round-robin)。

### <a name="create-a-load-balancer"></a>建立負載平衡器  
1. 建立 Azure Load Balancer   
      1. 在 Azure 入口網站中，按一下 [瀏覽 > 負載平衡器 > 新增]  。   
      2. 輸入新負載平衡器的名稱 (例如 hacb)。   
      3. 針對 [配置]  選取 [內部]  、針對您的部署選取 [虛擬網路]  (例如 Contoso-VNet)，並選取您所有資源的 [子網路]  (例如，預設值)。   
      4. 針對 [IP 位址指派]  選取 [靜態]  ，然後輸入目前非使用中的 [私人 IP 位址]  (例如 10.0.0.32)。   
      5. 選取適當的 [訂用帳戶]  、包含您所有資源的 [資源群組]  ，以及適當的 [位置]  。   
      6. 選取 [建立]  。   
2. 建立[探查](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/)以監視作用中的伺服器：   
      1. 在 Azure 入口網站中，按一下 [瀏覽 > 負載平衡器]  ，然後按一下您剛建立的負載平衡器 (例如 CBLB)。 按一下 [設定]  。   
      2. 按一下 [探查 > 新增]  。   
      3. 輸入探查的名稱 (例如 **RDP**)、選取 [TCP]  作為 [通訊協定]  、針對 [連接埠]  輸入 **3389**，然後按一下 [確定]  。   
3. 建立連線代理人的後端集區：   
      1. 在 [設定]  中，按一下 [後端位址集區 > 新增]  。   
      2. 輸入名稱 (例如 CBBackendPool)，然後按一下 [新增虛擬機器]  。  
      3. 選擇可用性設定組 (例如 CbAvSet)，然後按一下 [確定]  。   
      3. 按一下 [選擇虛擬機器]  ，選取每個虛擬機器，然後按一下 [選取 > 確定 > 確定]  。   
4. 建立 RDP 負載平衡規則：   
      1. 在 [設定]  中按一下 [負載平衡規則]  ，然後按一下 [新增]  。   
      2. 輸入名稱 (例如 RDP)、選取 [TCP]  作為 [通訊協定]  、針對 [連接埠]  和 [後端連接埠]  都輸入 **3389**，然後按一下 [確定]  。   
5. 新增負載平衡器的 DNS 記錄：   
      1. 連線至 RDMS 伺服器虛擬機器 (例如 Contoso-CB1)。 如需連線至 VM 的步驟，請參閱[準備 RD 連線代理人 VM](Prepare-the-RD-Connection-Broker-VM-for-Remote-Desktop.md) 一文。   
      2. 在 [伺服器管理員] 中，按一下 [工具 > DNS]  。   
      3. 在左窗格中，展開 [DNS]  、按一下 DNS 機器、按一下 [正向對應區域]  ，然後按一下您的網域名稱 (例如 Contoso.com)。 (處理向 DNS 伺服器取得資訊的查詢可能需要幾秒鐘的時間。)  
      4. 按一下 [動作 > 新增主機 (A 或 AAAA)]  。   
      9. 輸入名稱 (例如 hacb) 和先前指定的 IP 位址 (例如 10.0.0.32)。   

### <a name="configure-dns-round-robin"></a>設定 DNS 循環配置資源  
  
下列步驟是建立 Azure 內部負載平衡器的替代方式。   
  
1. 連線至 Azure 入口網站中的 RDMS 伺服器。 使用遠端桌面連線用戶端   
2. 建立 DNS 記錄：   
      1. 在 [伺服器管理員] 中，按一下 [工具 > DNS]  。   
      2. 在左窗格中，展開 [DNS]  、按一下 DNS 機器、按一下 [正向對應區域]  ，然後按一下您的網域名稱 (例如 Contoso.com)。 (處理向 DNS 伺服器取得資訊的查詢可能需要幾秒鐘的時間。)  
      3. 按一下 [動作]  和 [新增主機 (A 或 AAAA)]  。   
      4. 輸入 RD 連線代理人叢集的 [DNS 名稱]  (例如 hacb)，然後輸入第一個 RD 連線代理人的 [IP 位址]  。   
      5. 為每個額外的記錄分別提供唯一的 IP 位址，對每個額外的 RD 連線代理人重複步驟 3 到 4。


例如，如果兩個 RD 連線代理人虛擬機器的 IP 位址分別為 10.0.0.8 和 10.0.0.9，您將會建立兩個 DNS 主機記錄：
 - 主機名稱：hacb.contoso.com，IP 位址：10.0.0.8
 - 主機名稱：hacb.contoso.com，IP 位址：10.0.0.9

## <a name="step-3-configure-the-connection-brokers-for-high-availability"></a>步驟 3：設定連線代理人的高可用性

1. 將新的 RD 連線代理人伺服器新增至伺服器管理員：
   1. 在 [伺服器管理員] 中，按一下 [管理 > 新增伺服器]  。
   2. 按一下 [立即尋找]  。
   3. 按一下新建立的 RD 連線代理人伺服器 (例如 Contoso-Cb2)，然後按一下 [確定]  。
2. 設定 RD 連線代理人的高可用性：
   1. 在 [伺服器管理員] 中，按一下 [遠端桌面服務 > 概觀]  。
   2. 以滑鼠右鍵按一下 [RD 連線代理人]  ，然後按一下 [設定高可用性]  。
   3. 逐頁操作精靈，直到進入 [設定類型] 區段。 選取 [共用資料庫伺服器]  ，然後按 [下一步]  。
   4. 輸入 RD 連線代理人叢集的 DNS 名稱。
   5. 輸入 SQL DB 的連接字串，然後逐頁操作精靈以建立高可用性。
3. 將新的 RD 連線代理人新增至部署
   1. 在 [伺服器管理員] 中，按一下 [遠端桌面服務 > 概觀]  。
   2. 以滑鼠右鍵按一下 RD 連線代理人，然後按一下 [新增 RD 連線代理人伺服器]  。
   3. 逐頁操作精靈直到進入 [伺服器選取項目]，然後選取新建立的 RD 連線代理人伺服器 (例如 Contoso-CB2)。
   4. 接受預設值，完成精靈。
4. 設定 RD 連線代理人伺服器和用戶端上信任的憑證。


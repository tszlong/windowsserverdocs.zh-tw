---
title: 新增 RD 連線代理人伺服器設定 rds 的高可用性
description: 了解如何將高可用性的 RDS 部署中的 RD 連線代理人。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 2f4fc63c6ff7c1254fda630a8f34188d8fedc8e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825039"
---
# <a name="add-the-rd-connection-broker-server-to-the-deployment-and-configure-high-availability"></a>將 RD 連線代理人伺服器加入部署並設定高可用性

>適用於：Windows Server （半年通道），Windows Server 2016

您可以部署遠端桌面連線代理人 （RD 連線代理人） 叢集，以提升您的遠端桌面服務基礎結構的延展和可用性。 

## <a name="pre-requisites"></a>必要條件

設定伺服器來做為第二個 RD 連線代理人-這可以是實體伺服器或 VM。

設定連線代理人的資料庫。 您可以使用[Azure SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-get-started/#create-a-new-aure-sql-database)或您的本機環境中的 SQL Server 執行個體。 我們討論使用 Azure SQL，但這些步驟仍適用於 SQL Server。 您要尋找資料庫的連接字串，並確定您有正確的 ODBC 驅動程式。

## <a name="step-1-configure-the-database-for-the-connection-broker"></a>步驟 1：設定資料庫的連線代理人

1. 尋找您所建立的資料庫連接字串-您需要這兩個識別您需要和更新版本中，當您設定連接訊息代理程式本身 （步驟 3），因此將字串儲存至其中您可以參考它輕鬆的 ODBC 驅動程式的版本。 以下是如何針對 Azure SQL 來尋找連接字串：  
    1. 在 Azure 入口網站中，按一下**瀏覽 > 資源群組**，按一下 資源群組，以供部署。   
    2. 選取您剛建立 （例如 CB DB1） 的 SQL 資料庫。   
    3. 按一下 **設定 > 屬性 > 顯示資料庫連接字串**。   
    4. 複製的連接字串**ODBC （包括 Node.js）**，這應該看起來像這樣：   
      
        Driver={SQL Server Native Client 13.0};Server=tcp:cb-sqls1.database.windows.net,1433;Database=CB-DB1;Uid=sqladmin@contoso;Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;   
  
    5. 「 Your_password_here"取代為實際的密碼。 連接至資料庫時，您將使用此整個字串，包含密碼，請使用。 
2. 在新的連接訊息代理程式上安裝的 ODBC 驅動程式： 
   1. 如果您要用於連線代理人的 VM，建立第一個 RD 連線代理人的公用 IP 位址。 （您只需要這樣做，如果 RDMS 虛擬機器還沒有公用 IP 位址允許透過 RDP 連線。）
       1. 在 Azure 入口網站中，按一下**瀏覽 > 資源群組**，按一下 部署的資源群組，然後按一下 第一部 RD 連線代理人虛擬機器 (例如，Contoso-Cb1)。
       2. 按一下 **設定 > 網路介面**，然後按一下 對應的網路介面。
       3. 按一下 **設定 > IP 位址**。
       4. 針對**公用 IP 位址**，選取**已啟用**，然後按一下**IP 位址**。
       5. 如果您有想要使用的現有公用 IP 位址，請從清單中選取。 否則，請按一下**新建**，輸入名稱，然後按一下  **確定** ，然後**儲存**。
   2. 連接到第一個 RD 連線代理人：
       1. 在 Azure 入口網站中，按一下**瀏覽 > 資源群組**，按一下 部署的資源群組，然後按一下 第一部 RD 連線代理人虛擬機器 (例如，Contoso-Cb1)。
       2. 按一下  **Connect > 開啟**若要開啟 遠端桌面用戶端。
       3. 在用戶端中，按一下**Connect**，然後按一下**使用另一個使用者帳戶**。 輸入網域系統管理員帳戶的使用者名稱和密碼。
       4. 按一下 **是**時看到有關憑證的警告。
   3. 下載[適用於 SQL Server 的 ODBC 驅動程式](https://www.microsoft.com/download/confirmation.aspx?id=50420)符合 ODBC 連接字串中的版本。 如上面的範例字串，我們需要安裝版本 13 的 ODBC 驅動程式。
   4. Sqlincli.msi 將檔案複製到第一部 RD 連線代理人伺服器。   
   5. 開啟 sqlincli.msi 檔案，並安裝原生用戶端。  
   6. 針對每個額外的 RD 連線代理人 (例如，Contoso-Cb2) 重複步驟 1-5。
   7. 將執行連線代理人的每部伺服器上安裝的 ODBC 驅動程式。

## <a name="step-2-configure-load-balancing-on-the-rd-connection-brokers"></a>步驟 2：RD 連線代理人上設定負載平衡 

如果您使用 Azure 基礎結構，您可以建立[Azure 負載平衡器](#create-a-load-balancer); 如果沒有，您可以設定最多[DNS 循環](#configure-dns-round--robin)。 

### <a name="create-a-load-balancer"></a>建立負載平衡器  
1. 建立 Azure Load Balancer   
      1. 在 Azure 入口網站中按一下**瀏覽 > 負載平衡器 > 新增**。   
      2. 輸入新負載平衡器 (例如 hacb) 的名稱。   
      3. 選取 **內部**如**配置**，**虛擬網路**為您的部署 (例如，Contoso 對 VNet)，而**子網路**所有您的資源 （例如，預設值）。   
      4. 選取 **靜態**for **IP 位址指派**，然後輸入**私人 IP 位址**也就是並非目前使用中 (例如 10.0.0.32)。   
      5. 選取適當**訂用帳戶**，則**資源群組**所有您的資源，與適當**位置**。   
      6. 選取 [建立]。   
2. 建立[探查](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/)要監視哪些伺服器正在作用：   
      1. 在 Azure 入口網站中，按一下**瀏覽 > 負載平衡器**，然後按一下您剛才建立的負載平衡器 (例如 CBLB)。 按一下 [設定]。   
      2. 按一下 **探查 > 新增**。   
      3. 輸入探查的名稱 (例如**RDP**)，選取**TCP**做為**通訊協定**，輸入**3389**如**的連接埠**，然後按一下 **[確定]**。   
3. 建立連接的代理程式後端集區：   
      1. 在 **設定**，按一下**後端位址集區 > 新增**。   
      2. 輸入的名稱 (例如 CBBackendPool)，然後按一下 **新增虛擬機器**。  
      3. 選擇可用性設定組 (例如，「 CbAvSet 」)，然後按一下**確定**。   
      3. 按一下 **選擇的虛擬機器**，選取 每部虛擬機器，然後按一下**選取 > 確定 > 確定**。   
4. 建立 RDP 負載平衡規則：   
      1. 在 **設定**，按一下**負載平衡規則**，然後按一下**新增**。   
      2. 輸入名稱 (例如 RDP) 中，選取**TCP** for**通訊協定**，輸入**3389**同時**連接埠**和**後端連接埠**，然後按一下 **[確定]**。   
5. 負載平衡器新增 DNS 記錄：   
      1. 連接到 RDMS server 虛擬機器 (例如，Contoso-CB1)。 請參閱[準備 RD 連線代理人 VM](Prepare-the-RD-Connection-Broker-VM-for-Remote-Desktop.md)文章的步驟，在連接到 VM 的方式。   
      2. 在 [伺服器管理員] 中，按一下**工具 > DNS**。   
      3. 在左側窗格中，依序展開**DNS**，按一下 DNS 機器，按一下**正向對應區域**，然後按一下 您的網域名稱 (例如，Contoso.com)。 （它可能需要幾秒鐘才會處理查詢到 DNS 伺服器的資訊）。  
      4. 按一下 **動作 > 新增主機 （A 或 AAAA）**。   
      9. 輸入名稱 (例如 hacb) 和稍早指定的 IP 位址 (例如 10.0.0.32)。   
  
### <a name="configure-dns-round-robin"></a>設定 DNS 循環  
  
下列步驟會建立 Azure 內部負載平衡器的替代方案。   
  
1. 連接到 Azure 入口網站中 RDMS 伺服器。 使用遠端桌面連線用戶端   
2. 建立 DNS 記錄：   
      1. 在 [伺服器管理員] 中，按一下**工具 > DNS**。   
      2. 在左側窗格中，依序展開**DNS**，按一下 DNS 機器，按一下**正向對應區域**，然後按一下 您的網域名稱 (例如，Contoso.com)。 （它可能需要幾秒鐘才會處理查詢到 DNS 伺服器的資訊）。  
      3. 按一下 **動作**並**新增主機 （A 或 AAAA）**。   
      4. 請輸入**DNS 名稱**RD 連線代理人叢集 （例如 hacb），並輸入**IP 位址**的第一個 RD 連線代理人。   
      5. 針對每個額外的 RD 連線代理人，為每個額外的記錄提供每個唯一的 IP 位址重複步驟 3 到 4。


例如，如果兩部 RD 連線代理人虛擬機器的 IP 位址 10.0.0.8 和 10.0.0.9，您會建立兩個 DNS 主機記錄：
 - 主機名稱： hacb.contoso.com、 IP 位址：10.0.0.8
 - 主機名稱： hacb.contoso.com、 IP 位址：10.0.0.9

## <a name="step-3-configure-the-connection-brokers-for-high-availability"></a>步驟 3：設定連線代理人高可用性

1. 將新的 RD 連線代理人伺服器新增到伺服器管理員：
   1. 在 [伺服器管理員] 中，按一下**管理 > 新增伺服器**。
   2. 按一下 **立即尋找**。
   3. 按一下新建立的 RD 連線代理人伺服器 (例如，Contoso-Cb2)，然後按一下**確定**。
2. 設定 RD 連線代理人高可用性：
   1. 在 [伺服器管理員] 中，按一下**遠端桌面服務 > 概觀**。
   2. 以滑鼠右鍵按一下**RD 連線代理人**，然後按一下**設定高可用性**。
   3. 執行精靈直到到達 [組態類型] 區段的頁面。 選取 [**共用資料庫伺服器**，然後按一下**下一步]**。
   4. 輸入 RD 連線代理人叢集 DNS 名稱。
   5. SQL db 中，輸入連接字串，然後透過建立高可用性精靈 頁面上。
3. 加入部署中的新 RD 連線代理人
   1. 在 [伺服器管理員] 中，按一下**遠端桌面服務 > 概觀**。
   2. RD 連線代理人，以滑鼠右鍵按一下，然後按一下**新增 RD 連線代理人伺服器**。
   3. 逐頁查看精靈直到到達 伺服器選取項目，然後選取新建立的 RD 連線代理人伺服器 (例如，Contoso-CB2)。
   4. 完成精靈，接受預設值。
4. 設定 RD 連線代理人伺服器和用戶端上的受信任的憑證。


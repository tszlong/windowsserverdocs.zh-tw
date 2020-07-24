---
title: 使用 SQL Server 複寫設定地理位置冗余
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: 44f1f4598762f3d6e08430aa39b7cfa89eae4460
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966590"
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>使用 SQL Server 複寫設定地理位置冗余


> [!IMPORTANT]  
> 如果您想要建立 AD FS 伺服器陣列，並使用 SQL Server 來儲存設定資料，您可以使用 SQL Server 2008 或更高版本。
  
如果您使用 SQL Server 作為 AD FS 設定資料庫，您可以 \- 使用 SQL Server 複寫為您的 AD FS 伺服器陣列設定地理位置冗余。 異地複寫 \- 會在兩個地理位置較遠的網站之間複寫資料，讓應用程式可以從某個網站切換至另一個網站。 如此一來，萬一某個網站失敗，您仍然可以在第二個網站上使用所有設定資料。 如需詳細資訊，請參閱[使用 SQL Server 的同盟伺服器](../design/Federation-Server-Farm-Using-SQL-Server.md)陣列中的「SQL Server 地理位置冗余」一節。  
  
## <a name="prerequisites"></a>先決條件  
安裝和設定 SQL server 伺服器陣列。 如需詳細資訊，請參閱 [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://www.microsoft.com/en-us/evalcenter/)。 在初始 SQL Server 上，確定 SQL Server Agent 服務正在執行中，並設定為 [自動啟動]。  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>建立異地冗余的第二個 \( 複本 \) SQL Server \-  
  
1. 安裝 SQL Server \( 如需詳細資訊，請參閱 [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://www.microsoft.com/en-us/evalcenter/) 。 將產生的 CreateDB 和 SetPermissions 複製到 sql server 複本。  
  
2. 確定 SQL Server Agent 服務正在執行，並設定為自動啟動  
  
3. 在主要 AD FS 節點上執行 [**匯出 \- AdfsDeploymentSQLScript** ]，以建立 CreateDB .sql 和 SetPermissions 檔案。  例如： `PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$` 。  
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql2.png)
  
4. 將腳本複製到次要伺服器。  在**sql Management Studio**中開啟 CreateDB 腳本，然後按一下 [**執行**]。
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql4.png)

5. 在**sql Management Studio**中開啟 SetPermissions 腳本，然後按一下 [**執行**]。
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql6.png) 

   

> [!NOTE]
> 您也可以從命令列使用下列程式碼。 
> 
>    `c:\>sqlcmd –i CreateDB.sql`  
> 
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
> ## <a name="create-publisher-settings-on-the-initial-sql-server"></a>在初始 SQL Server 上建立發行者設定  
  
1. 從 SQL Server Management studio 的 [複寫] 底下 **，以滑鼠**按右鍵 [本機發行集 **]** ，然後選擇 [**新增發行** 
    ![ 集 ...]。設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql7.png) </br>  

2. 在 [新增發行集] 畫面上，按 **[下一步]**。</br>
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql8.png) </br> 
  
3. 在 **[** 散發者] 頁面上，選擇 [本機伺服器] 做為散發者，然後按 **[**  
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql9.png) </br>   

4. 在 [**快照**集資料夾] 頁面上，輸入 \\ \SQL1\repldata 來取代預設資料夾。 \(注意：您可能必須自行建立此共用 \) 。  
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql10.png) </br>   
  
5. 選擇 [ **AdfsConfigurationV3** ] 作為發行集資料庫，然後按 **[下一步]**。  
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql11.png) </br>
  
6. 在**發行集類型**上，選擇**合併式發行**集，然後按 **[下一步]**  
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql12.png) </br>
  
7. 在 [**訂閱者類型**] 上選擇**SQL Server 2008 或更新版本**，然後按 **[下一步]**  
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql13.png) </br> 

8. 在 [發行項] 頁面**上，選取**[**資料表]** 節點以選取所有資料表，然後取消核取 [ ** \- SyncProperties** \( ] 資料表。\)</br>
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql14.png) </br>    
  
9. **在 [發行**項] 頁面上，選取 [**使用者定義函數**] 節點以選取所有使用者定義函數，然後按 **[下一步]**。  
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql15.png) </br>    

10. 在 [**發行項問題**] 頁面上按 **[下一步]**。  
    ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql16.png) </br>   

11. 在 [篩選資料表的資料列]**** 頁面上，按一下 [下一步]****。  
    ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql17.png) </br>   
12. 在 [**快照集代理程式**] 頁面上，選擇 [立即] 和 [14 天]，然後按 **[下一步]**。  
    ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql18.png) </br>   
    您可能需要為 SQL 代理程式建立網域帳戶。 使用為[網域帳戶 CONTOSO \\ SQLAGENT 設定 sql 登](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent)入中的步驟，為這個新的 AD 使用者建立 sql 登入，並指派特定的許可權。  
  
13. 在 [**代理程式安全性**] 頁面上，按一下 [**安全性設定**]，並輸入網域帳戶的使用者名稱密碼，而 \/ \( 不是 \) 針對 SQL 代理程式建立的 GMSA，然後按一下 **[確定]**。  按 [下一步]  。  
    ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql19.png) </br>  

14. 在 [ **Wizard 動作]** 頁面上，按 **[下一步]**。   
    ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql20.png) </br>

15. 在 [**完成嚮導]** 頁面上，輸入發行集的名稱，然後按一下 **[完成**]。 
    ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql21.png) </br>  

16. 建立發行集之後，您應該會看到 [成功] 的狀態。  按一下 [關閉] 。
    ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql22.png) </br>  

17. 回到 SQL Server Management Studio，以滑鼠右鍵按一下新的發行集，然後按一下 [**啟動複寫監視器**]。  
    ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql23.png) </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>在複本上建立訂用帳戶設定 SQL Server  
請確定您已在初始 SQL Server 上建立發行者設定（如上所述），然後完成下列程式：  
  
1. 在 [複本 SQL Server 上，從 SQL Server Management studio 的 [複寫] 底下，以滑鼠右鍵按一下 [**本機訂閱** **]，然後**選擇 [**新增訂**用帳戶]。![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql24.png) </br>  

2. 在 [**新增訂閱嚮導]** 頁面上，按 **[下一步]**。
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql25.png) </br>   
  
3. 在 [**發行**集] 頁面上，從下拉式選單中選取 [發行者]。  展開 [ **AdfsConfigurationV3** ]，然後選取上面所建立的發行集名稱，然後按 **[下一步]**。  
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql26.png) </br> 
  
4. 在 [**合併代理程式位置**] 頁面上，選取 [在**其訂閱者 \( 提取 \) 訂閱上執行每個代理程式**]，並按 [ \( \) **下一步]**  
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql27.png) </br> 這連同下列訂用帳戶類型，會決定衝突解決邏輯。 \(如需詳細資訊，請參閱偵測[及解決合併](/sql/relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution?view=sql-server-ver15)式複寫衝突。 </br>
 
5. 在 [**訂閱者**] 頁面上，選取**AdfsConfigurationV3**作為訂閱者資料庫，然後按 **[下一步]**。  
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql28.png) </br> 
  
6. 在 [**合併代理程式安全性**] 頁面上，按一下 [ **...** ]，然後使用省略號方塊輸入不是針對 SQL 代理程式建立之 GMSA 的網域帳戶使用者名稱和密碼， \( 然後按 \) **[下一步]**。
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql30.png) </br> 
  
7. 在 **[同步處理排程**] 上，選擇 [**連續執行**] 並按 **[下一步** 
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql31.png) </br> 
 
8. 在 [**初始化訂閱**] 上，按 **[下一步]**。  
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql32.png) </br> 
  
9. 在 [訂用帳戶**類型**] 中選擇 [**用戶端**]，然後按 **[**  
  
   此問題的含意記載于[這裡](/sql/relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution?view=sql-server-ver15)和[這裡](/sql/relational-databases/replication/subscribe-to-publications?view=sql-server-ver15)。  基本上，我們會採用簡單的「第一次發行者獲勝」衝突解決，而我們不需要重新發行到其他訂閱者。  
   ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql33.png) </br>
   
10. 在 [ **Wizard 動作]** 頁面上，確定已核取 [**建立訂閱**]，然後按 **[下一步]**。 
    ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql34.png) </br>

11. 在 [**完成嚮導]** 頁面上，按一下 **[完成**]。 
    ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql35.png) </br>

12. 一旦訂用帳戶完成建立程式，您應該會看到 [成功]。 按一下 [關閉] 。 
    ![設定地理位置冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql36.png) </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>驗證初始化和複寫的程式  
  
1.  在主要 SQL server 上，以滑鼠右鍵 \- 按一下 [複寫] 節點 **，** 然後按一下 [啟動複寫**監視器**]。  
  
2.  在 [複寫**監視器**] 中，按一下發行集。  
  
3.  在 [**所有訂閱**] 索引標籤上，以滑鼠右鍵按一下並**查看詳細資料**。  
  
    您應該能夠在初始複寫的 [**動作**] 底下看到許多專案。  
  
4.  此外，您可以查看 [ **SQL Server Agent \\ 作業**] 節點底下，查看 \( \) 排程執行發行集訂閱之作業的工作 \/ 。  只會顯示本機作業，因此請務必檢查發行者和訂閱者以進行疑難排解。  以滑鼠右鍵 \- 按一下作業，然後選取 [ **view History** ] 以查看執行歷程記錄和結果。  
  
## <a name="configure-sql-login-for-the-domain-account-contososqlagent"></a><a name="sqlagent"></a>設定網域帳戶的 SQL 登入 CONTOSO \\ sqlagent  
  
1.  在主要和複本 SQL Server 上建立新的登入，稱為 CONTOSO \\ sqlagent 在 \( 上述程式的 [**代理程式安全性**] 頁面上建立和設定的新網域使用者名稱。\)  
  
2.  在 SQL Server 中，以滑鼠右鍵 \- 按一下您建立的登入，然後選取 [屬性]，然後在 [**使用者對應**] 索引標籤下，將此登入對應至**AdfsConfiguration** ，並**AdfsArtifact**具有 public 和 db \_ genevaservice 角色的資料庫 此外，也會將此登入對應到散發資料庫，並 \_ 為散發和 adfsconfiguration 資料表加入 db 擁有者角色。  請在主要和複本 SQL server 上都適用此作法。 如需詳細資訊，請參閱 [複寫代理程式安全性模型](/sql/relational-databases/replication/security/replication-agent-security-model?view=sql-server-ver15)。  
  
3.  將設定為「散發者」之共用的讀取和寫入權限授與對應的網域帳戶。  請務必在 [共用許可權] 和 [本機檔案] 許可權上設定 [讀取] 和 [寫入] 許可權。  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>將 AD FS 節點 \( 設定 \) 為指向 SQL Server 複本伺服器陣列  
現在您已設定地理位置冗余，可以使用標準 AD FS 的「加入」伺服器陣列功能，從 [AD FS] [設定] [Wizard] UI 或使用 Windows PowerShell，將 AD FS 伺服器陣列節點設定為指向您的複本 SQL Server 服務器陣列。  
  
如果您使用 [AD FS Configuration Wizard] UI，請選取 [**將同盟伺服器新增至同盟伺服器**陣列]。 **請勿**選取 **[在同盟伺服器陣列中建立第一部同盟伺服器**]。  
  
如果您使用 Windows PowerShell，請執行 [**新增 \- add-adfsfarmnode**]。 **請勿執行**[**安裝 \- AdfsFarm**]。  
  
出現提示時，請提供複本 SQL Server 的主機和實例名稱，而**不**是初始 SQL Server。  

---
title: 使用 SQL Server 複寫設定地理位置冗余
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: 16cf1a237043aa546d4fc24164045aa9f9a1e6ac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359825"
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>使用 SQL Server 複寫設定地理位置冗余


> [!IMPORTANT]  
> 如果您想要建立 AD FS 伺服器陣列，並使用 SQL Server 來儲存設定資料，您可以使用 SQL Server 2008 或更高版本。
  
如果您使用 SQL Server 作為 AD FS 設定資料庫，您可以使用 SQL Server 複寫為您的 AD FS 伺服器陣列設定異地 @ no__t-0redundancy。 異地 @ no__t-0redundancy 會在兩個地理位置較遠的網站之間複寫資料，讓應用程式可以從某個網站切換至另一個網站。 如此一來，萬一某個網站失敗，您仍然可以在第二個網站上使用所有設定資料。 如需詳細資訊，請參閱[使用 SQL Server 的同盟伺服器](../design/Federation-Server-Farm-Using-SQL-Server.md)陣列中的「SQL Server 地理位置冗余」一節。  
  
## <a name="prerequisites"></a>必要條件  
安裝和設定 SQL server 伺服器陣列。 如需詳細資訊, [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx)請參閱。 在初始 SQL Server 上，確定 SQL Server Agent 服務正在執行中，並設定為 [自動啟動]。  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>建立異地 @ no__t 的第二個 \(replica @ no__t-1 SQL Server  
  
1. 安裝 SQL Server \(for 詳細資訊，請參閱[https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx)。 將產生的 CreateDB 和 SetPermissions 複製到 sql server 複本。  
  
2. 確定 SQL Server Agent 服務正在執行，並設定為自動啟動  
  
3. 在主要 AD FS 節點上執行**Export @ no__t-1AdfsDeploymentSQLScript** ，以建立 CreateDB 和 SetPermissions .sql 檔案。  例如： `PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`。  
   ![Set 的地理複本 @ no__t-1
  
4. 將腳本複製到次要伺服器。  在**sql Management Studio**中開啟 CreateDB 腳本，然後按一下 [**執行**]。
   ![Set 的地理複本 @ no__t-1

5. 在**sql Management Studio**中開啟 SetPermissions 腳本，然後按一下 [**執行**]。
   ![Set 的地理複本 @ no__t-1 

   

> [!NOTE]
> 您也可以從命令列使用下列程式碼。 
> 
>    `c:\>sqlcmd –i CreateDB.sql`  
> 
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
> ## <a name="create-publisher-settings-on-the-initial-sql-server"></a>在初始 SQL Server 上建立發行者設定  
  
1. 從 SQL Server Management studio 的 [複寫] 底下 **，以滑鼠**按右鍵 [本機發行集 **]** ，然後選擇 [**新增發行**集 ...]。
    @ no__t-4Set 地理位置冗余 @ no__t-5 </br>  

2. 在 [新增發行集] 畫面上，按 **[下一步]** 。</br>
   ![Set 的地理複本 @ no__t-1 </br> 
  
3. 在散發者 頁面上，選擇 本機伺服器 做為散發者，然後按  
   ![Set 的地理複本 @ no__t-1 </br>   

4. 在 [**快照**集資料夾] 頁面上，輸入 \\ \ SQL1\repldata 來取代預設資料夾。 \(附註：您可能必須自行建立此共用 @ no__t-0。  
   ![Set 的地理複本 @ no__t-1 </br>   
  
5. 選擇 [ **AdfsConfigurationV3** ] 作為發行集資料庫，然後按 **[下一步]** 。  
   ![Set 的地理複本 @ no__t-1 </br>
  
6. 在**發行集類型**上，選擇**合併式發行**集，然後按 **[下一步]**  
   ![Set 的地理複本 @ no__t-1 </br>
  
7. 在 [**訂閱者類型**] 上選擇**SQL Server 2008 或更新版本**，然後按 **[下一步]**  
   ![Set 的地理複本 @ no__t-1 </br> 

8. 在 發行項 頁面**上，選取** **資料表**節點來選取所有資料表，然後**取消 no__t-3check SyncProperties**資料表 \(this 不應複寫 @ no__t-5</br>
   ![Set 的地理複本 @ no__t-1 </br>    
  
9. **在 [發行**項] 頁面上，選取 [**使用者定義函數**] 節點以選取所有使用者定義函數，然後按 **[下一步]** 。  
   ![Set 的地理複本 @ no__t-1 </br>    

10. 在 [**發行項問題**] 頁面上按 **[下一步]** 。  
    ![Set 的地理複本 @ no__t-1 </br>   

11. 在 [**篩選資料表資料列**] 頁面上，按 **[下一步]** 。  
    ![Set 的地理複本 @ no__t-1 </br>   
12. 在 [**快照集代理程式**] 頁面上，選擇 [立即] 和 [14 天]，然後按 **[下一步]** 。  
    ![Set 的地理複本 @ no__t-1 </br>   
    您可能需要為 SQL 代理程式建立網域帳戶。 使用[設定網域帳戶的 sql 登入 CONTOSO @ no__t-1sqlagent](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent)中的步驟，為這個新的 AD 使用者建立 sql 登入，並指派特定的許可權。  
  
13. 在 [**代理程式安全性**] 頁面上，按一下 [**安全性設定**]，然後輸入網域帳戶的使用者名稱 @ no__t-2password，\(not 為 SQL 代理程式建立的 GMSA @ no__t-4，然後按一下 **[確定]** 。  按一下 [下一步]。  
    ![Set 的地理複本 @ no__t-1 </br>  

14. 在 [ **Wizard 動作]** 頁面上，按 **[下一步]** 。   
    ![Set 的地理複本 @ no__t-1 </br>

15. 在 [**完成嚮導]** 頁面上，輸入發行集的名稱，然後按一下 **[完成**]。 
    ![Set 的地理複本 @ no__t-1 </br>  

16. 建立發行集之後，您應該會看到 [成功] 的狀態。  按一下 **關閉**。
    ![Set 的地理複本 @ no__t-1 </br>  

17. 回到 SQL Server Management Studio，以滑鼠右鍵按一下新的發行集，然後按一下 [**啟動複寫監視器**]。  
    ![Set 的地理複本 @ no__t-1 </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>在複本上建立訂用帳戶設定 SQL Server  
請確定您已在初始 SQL Server 上建立發行者設定（如上所述），然後完成下列程式：  
  
1. 在 複本 SQL Server 上，從 SQL Server Management studio 的 [複寫] 底下，以滑鼠右鍵按一下 [**本機訂閱** **]，然後**選擇 [**新增訂**用帳戶]。![Set 的地理複本 @ no__t-1 </br>  

2. 在 [**新增訂閱嚮導]** 頁面上，按 **[下一步]** 。
   ![Set 的地理複本 @ no__t-1 </br>   
  
3. 在 [**發行**集] 頁面上，從下拉式選單中選取 [發行者]。  展開 [ **AdfsConfigurationV3** ]，然後選取上面所建立的發行集名稱，然後按 **[下一步]** 。  
   ![Set 的地理複本 @ no__t-1 </br> 
  
4. 在 **合併代理程式位置** 頁面上，選取 在**訂閱者端執行每個代理程式 \(pull 訂用帳戶 @ no__t-3** \(the 預設 @ no__t-5，然後按**下一步**。  
   ![Set 的地理複本 @ no__t-1 </br> 這連同下列訂用帳戶類型，會決定衝突解決邏輯。 \(For 詳細資訊，請參閱偵測[及解決合併](https://technet.microsoft.com/library/ms151191.aspx)式複寫衝突。 </br>
 
5. 在 [**訂閱者**] 頁面上，選取**AdfsConfigurationV3**作為訂閱者資料庫，然後按 **[下一步]** 。  
   ![Set 的地理複本 @ no__t-1 </br> 
  
6. 在 [**合併代理程式安全性**] 頁面上，按一下 [ **...** ]，然後輸入網域帳戶的使用者名稱和密碼 @no__t-使用省略號方塊為 SQL 代理程式建立的 GMSA @ no__t-3，然後按 **[下一步]** 。
   ![Set 的地理複本 @ no__t-1 </br> 
  
7. 在**同步處理排程** 上，選擇 **連續執行** 並按**下一步** 
   ![Set 的地理複本 @ no__t-1 </br> 
 
8. 在 [**初始化訂閱**] 上，按 **[下一步]** 。  
   ![Set 的地理複本 @ no__t-1 </br> 
  
9. 在 訂用帳戶**類型** 中選擇 **用戶端**，然後按  
  
   此問題的含意記載于[這裡](https://technet.microsoft.com/library/ms151191.aspx)和[這裡](https://technet.microsoft.com/library/ms151170.aspx)。  基本上，我們會採用簡單的「第一次發行者獲勝」衝突解決，而我們不需要重新發行到其他訂閱者。  
   ![Set 的地理複本 @ no__t-1 </br>
   
10. 在 [ **Wizard 動作]** 頁面上，確定已核取 [**建立訂閱**]，然後按 **[下一步]** 。 
    ![Set 的地理複本 @ no__t-1 </br>

11. 在 [**完成嚮導]** 頁面上，按一下 **[完成**]。 
    ![Set 的地理複本 @ no__t-1 </br>

12. 一旦訂用帳戶完成建立程式，您應該會看到 [成功]。 按一下 **關閉**。 
    ![Set 的地理複本 @ no__t-1 </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>驗證初始化和複寫的程式  
  
1.  在主要 SQL server 上，right @ no__t-0click 複寫**節點，** 然後按一下 [**啟動複寫監視器**]。  
  
2.  在 [複寫**監視器**] 中，按一下發行集。  
  
3.  在 [**所有訂閱**] 索引標籤上，以滑鼠右鍵按一下並**查看詳細資料**。  
  
    您應該能夠在初始複寫的 [**動作**] 底下看到許多專案。  
  
4.  此外，您可以查看 [ **SQL Server Agent @ no__t-1Jobs** ] 節點底下，查看作業 @ no__t-2-2-3 已排程執行發行集 @ no__t-4subscription 的操作。  只會顯示本機作業，因此請務必檢查發行者和訂閱者以進行疑難排解。  Right @ no__t-0click 作業並選取 [**查看歷程記錄**] 來查看執行歷程記錄和結果。  
  
## <a name="sqlagent"></a>設定網域帳戶的 SQL 登入 CONTOSO @ no__t-1sqlagent  
  
1.  在上述程式的 [**代理程式安全性**] 頁面上，建立名為 CONTOSO @ no__t-0sqlagent 的主要和複本 SQL Server 的新登入 \(the 所建立和設定的新網域使用者名稱。 \)  
  
2.  在 SQL Server 中，right @ no__t-0click 您所建立的登入，然後選取 [屬性]，然後在 [**使用者對應**] 索引標籤下，將此登入對應到**AdfsConfiguration** ，並使用 public 和 db @ no__t-4genevaservice 角色來**AdfsArtifact**資料庫。 此外，也會將此登入對應到散發資料庫，並為散發和 adfsconfiguration 資料表加入 db @ no__t-0owner 角色。  請在主要和複本 SQL server 上都適用此作法。 如需詳細資訊，請參閱[Replication Agent Security Model](https://technet.microsoft.com/library/ms151868.aspx)。  
  
3.  將設定為「散發者」之共用的讀取和寫入權限授與對應的網域帳戶。  請務必在 [共用許可權] 和 [本機檔案] 許可權上設定 [讀取] 和 [寫入] 許可權。  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>設定 AD FS node @ no__t-0-0 @ no__t-1 以指向 SQL Server 複本伺服器陣列  
現在您已設定地理位置冗余，可以使用標準 AD FS 的「加入」伺服器陣列功能，從 [AD FS] [設定] [Wizard] UI 或使用 Windows PowerShell，將 AD FS 伺服器陣列節點設定為指向您的複本 SQL Server 服務器陣列。  
  
如果您使用 [AD FS Configuration Wizard] UI，請選取 [**將同盟伺服器新增至同盟伺服器**陣列]。 **請勿**選取 **[在同盟伺服器陣列中建立第一部同盟伺服器**]。  
  
如果您使用 Windows PowerShell，請執行**Add @ no__t-1AdfsFarmNode**。 **請勿**執行**Install @ no__t-2AdfsFarm**。  
  
出現提示時，請提供複本 SQL Server 的主機和實例名稱，而**不**是初始 SQL Server。  

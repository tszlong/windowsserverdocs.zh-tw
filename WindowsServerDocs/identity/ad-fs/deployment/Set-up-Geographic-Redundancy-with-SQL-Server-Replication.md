---
title: "設定地理冗餘的 SQL Server 複寫"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: a9f29c1eb19a8241eac5afb5c87581e6c988c3c8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>設定地理冗餘的 SQL Server 複寫

>適用於：Windows Server 2016、Windows Server 2012 R2

> [!IMPORTANT]  
> 如果您想要建立 AD FS 發電廠 SQL Server 來儲存您設定的資料的使用，您可以使用 SQL Server 2008，或更高版本。
  
如果您正在使用 SQL Server 做 AD FS 設定資料庫，您可以設定為使用 SQL Server 複寫您 AD FS 發電廠 geo\-冗餘。 Geo\-冗餘複寫之間兩個地理位置伸到遠端的網站資料，讓應用程式可以切換到另一個網站。 如此一來，某個網站故障，您仍然會所有設定可用的資料在第二個網站。 如需詳細資訊，在中看到 「 SQL Server 地理冗餘區段 」[聯盟伺服器發電廠使用 SQL Server](../design/Federation-Server-Farm-Using-SQL-Server.md)。  
  
## <a name="prerequisites"></a>必要條件  
安裝和設定 SQL server 發電廠。 如需詳細資訊，請查看[https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx)。 在初始 SQL Server 中，確定 SQL Server 代理程式服務正在執行，設定為自動 [開始] 畫面。  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>建立 geo\-冗餘的第二個 \(replica\) SQL Server  
  
1.  安裝 SQL Server \ (如需詳細資訊，請查看[https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx)。 顯示 CreateDB.sql 和 SetPermissions.sql 指令碼將檔案複製到複本 SQL server。  
  
2.  確定服務 SQL Server 代理程式正在執行，並將設定為自動 [開始] 畫面  
  
3.  執行**Export\-AdfsDeploymentSQLScript**來建立 CreateDB.sql 和 SetPermissions.sql 檔案的主要 AD FS 節點上。  例如：`PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`。  
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql2.png)
  
4.  複製到您的第二個伺服器的指令碼。  打開 CreateDB.sql 指令碼，在**SQL 管理 Studio** ，按一下 [**執行**。
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql4.png)

5. 打開 SetPermissions.sql 指令碼以在**SQL 管理 Studio** ，按一下 [**執行**。
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql6.png) 

   

>[!NOTE]
>您也可以使用下列命令。 
>
>    `c:\>sqlcmd –i CreateDB.sql`  
>  
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
## <a name="create-publisher-settings-on-the-initial-sql-server"></a>在初始 SQL Server 建立發行者設定  
  
1.  SQL Server 管理 studio，在**複製**，以滑鼠右鍵按一下**本機發行**，然後選擇 [**新發行...**
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql7.png) </br>  

2.  在畫面上新的發行精靈按一下**下一步**。</br>
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql8.png) </br> 
  
3.  在**代理商**頁面上，選擇 [本機伺服器代理商，按一下 [**下**。  
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql9.png) </br>   

4.  在**快照**資料夾頁面上，輸入 \\\SQL1\repldata 來取代預設的資料夾。 \ (請注意： 您必須建立此分享 yourself\)。  
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql10.png) </br>   
  
5.  選擇 [ **AdfsConfigurationV3**發行資料庫和按**下**。  
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql11.png) </br>
  
6.  在**發行輸入**，選擇**合併發行**並按**下一步**。  
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql12.png) </br>
  
7.  在**訂戶類型**，選擇**SQL Server 2008 或更新版本**並按**下一步**。  
 ![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql13.png) </br> 

8.  在**文章**頁面選取**表格**節點，然後選取 [所有表格， **un\ 檢查 SyncProperties**表格 \ （此不應該 replicated\）</br>
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql14.png) </br>    
  
9.  在**文章**頁面上，選取**定義函式使用者**節點選取所有使用者定義函式並按一下 [**下一步**...  
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql15.png) </br>    

10. 在**文章問題**頁面上按**下一步**。  
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql16.png) </br>   

11. 在**篩選表格列**頁面上，按一下 [**下**。  
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql17.png) </br>   
12. 在**快照代理程式**頁面上，選擇預設值的即時和 14 天後，按**下**。  
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql18.png) </br>   
您可能需要建立核對 SQL 代理程式。 使用中的步驟執行[設定 SQL 核對 CONTOSO\\sqlagent 登入](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent)來建立 SQL 這個新的廣告使用者登入並指定特定的權限。  
  
13. 在**代理程式安全性**頁面上，按一下 [**的安全性設定**輸入核對 username\ 日密碼 \ (不 GMSA\) 建立 SQL 代理程式，按一下 [ **[確定]**。  按一下**下一步**。  
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql19.png) </br>  

14. 在**精靈動作**頁面上，按一下 [**下**。   
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql20.png) </br>

15. 在**完成精靈**頁面上輸入物的名稱，然後按一下**完成]**。 
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql21.png) </br>  

16. 一旦建立發行之後，您應該會看到成功的狀態。  按一下**關閉**。
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql22.png) </br>  

17. 返回 SQL Server 管理 studio，以滑鼠右鍵按一下新的發行，然後按一下**上市複寫監視器**。  
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql23.png) </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>複本 SQL Server 上建立的設定  
請確定您在初始 SQL Server 上建立的發行者設定，如上文所述，然後完成下列程序：  
  
1.  在複本 SQL Server SQL Server 管理 studio，在**複製**，以滑鼠右鍵按一下**本機月租方案**，然後選擇 [**新裝機費...**.![設定地理冗餘](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql24.png) </br>  

2.  在**新裝機費精靈**頁面上，按一下 [**下**。
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql25.png) </br>   
  
3.  在**發行**頁面上，從下拉式清單中選取發行者。  展開**AdfsConfigurationV3**並選取 [建立上述發行的名稱，然後按一下 [**下**。  
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql26.png) </br> 
  
4.  在**合併代理程式位置**頁面上，選取**執行每個代理程式其訂戶 \(pull subscriptions\)** \(the default\)，按一下 [**下一步**。  
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql27.png) </br> 以及裝機費輸入下列判斷衝突解析度邏輯操作。 \ (如需詳細資訊，請查看[偵測與解析合併複寫衝突](https://technet.microsoft.com/library/ms151191.aspx)。 </br>
 
5.  在**訂閱**頁面上，選取**AdfsConfigurationV3**為訂戶資料庫，並按**下一步**。  
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql28.png) </br> 
  
6.  在**合併代理程式安全性**頁面上，按**...** ，並輸入的使用者名稱和密碼核對 \ (不 GMSA\) SQL 代理程式建立使用針對方塊與，按一下 [**下一步**。
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql30.png) </br> 
  
7.  在**同步排程**，選擇**持續執行**並按**下一步**。 
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql31.png) </br> 
 
8.  在**初始化月租方案**，按一下 [**下**。  
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql32.png) </br> 
  
9.  在**裝機費類型**，選擇**Client**並按**下一步**。  
  
    記載的影響的[在此](https://technet.microsoft.com/library/ms151191.aspx)和[以下](https://technet.microsoft.com/library/ms151170.aspx)。  基本上，我們需要簡單 」 第一次發行者 wins 「 衝突，我們就不需要重新其他訂閱。  
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql33.png) </br>
   
10. 在**精靈動作**頁面上，請確定**建立裝機費**核取，並按一下 [**下一步**。 
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql34.png) </br>

11. 在**完成精靈**頁面上，按**完成]**。 
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql35.png) </br>

12. 一旦建立程序完成裝機費，您應該會看到成功。 按一下**關閉**。 
![地理冗餘設定](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql36.png) </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>請確認初始設定和複寫程的序  
  
1.  主要 SQL server 上 right\ 按一下**複製**節點，然後按一下 [**上市複寫監視器**。  
  
2.  在**複寫監視器**，按一下 [發行。  
  
3.  在**所有月租方案**索引標籤上，以滑鼠右鍵按一下和**檢視詳細資料**。  
  
    您應該會看到許多項目在**動作**的初始複寫。  
  
4.  此外，您可以檢視在**SQL Server Agent\\Jobs**節點，以查看 job\(s\) 排程來執行的作業 publication\ 日裝機費。  只顯示本機工作，所以請務必檢查以取得疑難排解發行者及訂戶。  Right\ 按一下工作，然後選取**檢視歷史**檢視中執行歷史及結果。  
  
## <a name="sqlagent"></a>設定針對網域 account CONTOSO\\sqlagent SQL 登入  
  
1.  建立新的登入主要的及複本稱為 CONTOSO\\sqlagent SQL Server \ (建立新的網域使用者名稱，並設定**代理程式安全性**頁面中的程序上述。 \)  
  
2.  在 SQL Server 中 right\ 按一下 [登入一經建立，並選取 []，然後在您**使用者對應**索引標籤上，地圖此登入**AdfsConfiguration**並**AdfsArtifact**的公開和 db\_genevaservice 角色資料庫。 也 distribution 資料庫地圖此登入並加入 adfsconfiguration 表格和 distribution db\_owner 角色。  這樣做為主要和複本 SQL server 適用。 如需詳細資訊，請查看[複寫專員安全性模型](https://technet.microsoft.com/library/ms151868.aspx)。  
  
3.  提供相對應的網域 account 讀取和寫入設定為代理商共用的權限。  請確定您設定讀取和寫入同時共用權限和本機的檔案權限的權限。  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>設定 AD FS node\(s\) 指向 SQL Server 複本陣列  
既然您已經設定地理冗餘，AD FS 發電廠節點可以指向使用標準 AD FS 」 加入 「 發電廠功能，其中一個 AD FS 設定精靈 ui 或使用 Windows PowerShell 您複本 SQL Server 發電廠設定。  
  
如果您使用 AD FS 設定精靈 UI，選取 [**新增聯盟伺服器聯盟伺服器陣列到**。 **不要**選擇**的第一個聯盟伺服器建立聯盟伺服器陣列**。  
  
如果您使用 Windows PowerShell，執行**Add\-AdfsFarmNode**。 **不要**執行**-AdfsFarm Install\ 的**。  
  
出現提示時，提供主機和執行個體名稱複本 SQL Server 的**未**初始 SQL server。  

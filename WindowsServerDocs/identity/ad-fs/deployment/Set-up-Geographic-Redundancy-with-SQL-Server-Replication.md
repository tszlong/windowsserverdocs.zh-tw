---
title: 安裝程式的異地備援，使用 SQL Server 複寫
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: cf3d7513dd02bdb578bffd2f3ef8bdb29d8f983d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192175"
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>安裝程式的異地備援，使用 SQL Server 複寫


> [!IMPORTANT]  
> 如果您想要建立 AD FS 伺服器陣列，並使用 SQL Server 來儲存設定資料，您可以使用 SQL Server 2008 或更新版本。
  
如果您使用 SQL Server 做為您的 AD FS 設定資料庫，您可以設定異地\-AD FS 伺服器陣列使用 SQL Server 複寫的備援。 異地\-備援之間複寫資料兩個地理位置遙遠的站台，讓應用程式可以依序切換到另一個站台。 如此一來，萬一發生的一個站台失敗，您仍可以提供所有組態資料在第二個站台。 如需詳細資訊，請參閱 「 SQL Server 的異地備援區段 」 中[同盟伺服器陣列使用 SQL Server](../design/Federation-Server-Farm-Using-SQL-Server.md)。  
  
## <a name="prerequisites"></a>必要條件  
安裝和設定 SQL server 伺服器陣列。 如需詳細資訊，請參閱 < [ https://technet.microsoft.com/evalcenter/hh225126.aspx ](https://technet.microsoft.com/evalcenter/hh225126.aspx)。 在初始的 SQL Server，請確定 SQL Server Agent 服務正在執行，並設定為自動啟動。  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>建立第二個\(複本\)異地的 SQL Server\-備援  
  
1.  安裝 SQL Server\(如需詳細資訊，請參閱 < [ https://technet.microsoft.com/evalcenter/hh225126.aspx ](https://technet.microsoft.com/evalcenter/hh225126.aspx)。 產生 CreateDB.sql 和 SetPermissions.sql 指令碼檔案複製到複本的 SQL server。  
  
2.  請確定 SQL Server Agent 服務正在執行，並設定為自動啟動  
  
3.  執行**匯出\-AdfsDeploymentSQLScript**主要 AD FS 節點以建立 CreateDB.sql 和 SetPermissions.sql 檔案上。  例如：`PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`。  
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql2.png)
  
4.  將指令碼複製到次要伺服器。  開啟中的 CreateDB.sql 指令碼**SQL Management Studio**然後按一下**Execute**。
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql4.png)

5. 開啟中的 SetPermissions.sql 指令碼**SQL Management Studio**然後按一下**Execute**。
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql6.png) 

   

>[!NOTE]
>您也可以使用下列從命令列。 
>
>    `c:\>sqlcmd –i CreateDB.sql`  
>  
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
## <a name="create-publisher-settings-on-the-initial-sql-server"></a>建立初始的 SQL Server 上的 發行者設定  
  
1.  從 SQL Server Management studio 中的底下**複寫**，以滑鼠右鍵按一下**本機發行集**，然後選擇 **新增發行集...** 
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql7.png) </br>  

2.  在 新增發行集精靈 畫面上按一下**下一步** 。</br>
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql8.png) </br> 
  
3.  在 **散發者端**頁面上，選擇 本機伺服器為散發者 」，然後按一下**下一步** 。  
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql9.png) </br>   

4.  在 **快照集**資料夾頁面上，輸入\\\SQL1\repldata 取代預設的資料夾。 \(附註：您可能必須自行建立此共用\)。  
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql10.png) </br>   
  
5.  選擇**AdfsConfigurationV3**作為發行集資料庫，然後按一下**下一步** 。  
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql11.png) </br>
  
6.  在 [**發行集類型**，選擇**合併式發行集**然後按一下**下一步]** 。  
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql12.png) </br>
  
7.  在 **訂閱者類型**，選擇**SQL Server 2008 或更新版本**，按一下 **下一步** 。  
 ![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql13.png) </br> 

8.  上**發行項**頁面上，選取**資料表**節點以選取所有資料表，然後**取消\-檢查 SyncProperties**資料表\(應該不到這一個複寫\)</br>
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql14.png) </br>    
  
9.  在上**發行項**頁面上，選取**使用者定義函式**節點以選取所有使用者定義函式，然後按一下**下一步** ...  
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql15.png) </br>    

10. 在 [**發行項問題**頁面上，按一下**下一步]** 。  
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql16.png) </br>   

11. 在 [**篩選資料表的資料列**頁面上，按一下**下一步]** 。  
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql17.png) </br>   
12. 在 **快照集代理程式**頁面上，選擇 即時運算和 14 天的預設值，按一下**下一步** 。  
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql18.png) </br>   
您可能需要建立 SQL 代理程式的網域帳戶。 使用中的步驟[設定的 SQL 登入網域帳戶 CONTOSO\\sqlagent](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent)建立這個新的 AD 使用者的 SQL 登入並指派特定權限。  
  
13. 在上**代理程式安全性**頁面上，按一下**安全性設定**，然後輸入使用者名稱\/網域帳戶的密碼\(不 GMSA\)建立 SQL 代理程式和按一下 **確定**。  按一下 [下一步]  。  
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql19.png) </br>  

14. 在 [**精靈動作**頁面上，按一下**下一步]** 。   
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql20.png) </br>

15. 在 **完成精靈**頁面上，輸入您的發行集的名稱，然後按一下**完成**。 
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql21.png) </br>  

16. 建立發行集之後，您應該看到成功的狀態。  按一下 **關閉**。
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql22.png) </br>  

17. 傳回在 SQL Server Management Studio，請以滑鼠右鍵按一下新的發行集，然後按一下 **啟動複寫監視器**。  
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql23.png) </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>建立複本 SQL Server 上的訂用帳戶設定  
請確定您在初始的 SQL Server 上建立 「 發行者 」 設定，如上面所述，然後完成下列程序：  
  
1.  SQL Server Management studio 中，從 SQL Server 複本上底下**複寫**，以滑鼠右鍵按一下**本機訂用帳戶**，然後選擇 **新訂用帳戶...** .![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql24.png) </br>  

2.  在 [**新的訂用帳戶精靈**頁面上，按一下**下一步]** 。
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql25.png) </br>   
  
3.  在 **發行集**頁面上，從下拉式清單中選取 「 發行者 」。  依序展開**AdfsConfigurationV3**並選取上述建立的發行集的名稱，然後按一下**下一步** 。  
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql26.png) </br> 
  
4.  在 [**合併代理程式位置**頁面上，選取**在訂閱者端執行每個代理程式\(提取訂閱\)** \(預設\)按一下**下一步]** 。  
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql27.png) </br> 這項目，以及訂用帳戶類型，判斷衝突解決邏輯。 \(如需詳細資訊，請參閱 <<c0> [ 偵測並解決合併式複寫衝突](https://technet.microsoft.com/library/ms151191.aspx)。 </br>
 
5.  在 [**訂閱者**頁面上，選取**AdfsConfigurationV3**作為訂閱者資料庫，然後按一下**下一步]** 。  
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql28.png) </br> 
  
6.  在 [**合併代理程式安全性**頁面上，按一下 **...** ，然後輸入使用者名稱和網域帳戶的密碼\(不 GMSA\)建立 SQL 代理程式所使用的省略符號的方塊，然後按一下**下一步]** 。
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql30.png) </br> 
  
7.  在 [**同步處理排程**，選擇**連續執行**然後按一下**下一步]** 。 
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql31.png) </br> 
 
8.  在 [**初始化訂閱**，按一下**下一步]** 。  
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql32.png) </br> 
  
9.  在 [**訂用帳戶類型**，選擇**用戶端**然後按一下**下一步]** 。  
  
    記載背後的含意[此處](https://technet.microsoft.com/library/ms151191.aspx)並[這裡](https://technet.microsoft.com/library/ms151170.aspx)。  基本上，我們採用簡單的"first 發行者成功 」 衝突解決方法，我們不需要重新發行到其他訂閱者。  
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql33.png) </br>
   
10. 在 **精靈動作**頁面上，確定**建立訂用帳戶**核取，然後按一下 **下一步** 。 
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql34.png) </br>

11. 在 **完成精靈**頁面上，按一下**完成**。 
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql35.png) </br>

12. 在建立程序完成後的訂用帳戶，您應該會看到成功。 按一下 **關閉**。 
![設定地理備援](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql36.png) </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>請確認初始設定和複寫的程序  
  
1.  在主要 SQL 伺服器上，以滑鼠右鍵\-按一下 **複寫**節點，然後按一下**啟動複寫監視器**。  
  
2.  在 **複寫監視器**，按一下 發行集。  
  
3.  在 **所有訂用帳戶**索引標籤上，以滑鼠右鍵按一下並**檢視詳細資料**。  
  
    您應該能夠看到許多項目底下**動作**進行初始複寫。  
  
4.  此外，您可以查看底下**SQL Server Agent\\作業**節點以查看作業\(s\)排程執行作業的發行集\/訂用帳戶。  只會顯示本機作業，因此請務必檢查 「 發行者 」 和 「 訂閱者 」 上進行疑難排解。  右\-按一下 [工作]，然後選取**檢視歷程記錄**來檢視執行歷程記錄和結果。  
  
## <a name="sqlagent"></a>設定 SQL 帳戶登入網域 CONTOSO\\sqlagent  
  
1.  在主要和複本名為 CONTOSO 的 SQL Server 上建立新的登入\\sqlagent\(新的網域使用者的名稱上建立及設定**代理程式安全性**上述程序中的頁面。\)  
  
2.  在 SQL Server 中，以滑鼠右鍵\-按一下您所建立的登入，然後選取 內容，然後在**使用者對應**索引標籤上，對應到此登入**AdfsConfiguration**和**AdfsArtifact**資料庫使用公用和 db\_genevaservice 角色。 也將此登入對應至散發資料庫，並新增 db\_發佈和 adfsconfiguration 資料表的擁有者角色。  視情況在主要和複本的 SQL server 上執行這項操作。 如需詳細資訊，請參閱 < [Replication Agent Security Model](https://technet.microsoft.com/library/ms151868.aspx)。  
  
3.  提供對應的網域帳戶讀取和寫入權限設定為散發者 」 共用上。  請確定您設定讀取和寫入權限的共用權限和本機檔案的權限。  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>設定 AD FS 節點\(s\)指向 SQL Server 複本伺服器陣列  
既然您已設定異地備援，可以設定 AD FS 伺服器陣列節點，以指向您的複本 SQL Server 伺服器陣列使用的標準 AD FS [加入] 伺服器陣列功能，從 AD FS 設定精靈 UI，或使用 Windows PowerShell。  
  
如果您使用 AD FS 設定精靈 UI，請選取**新增至同盟伺服器陣列的同盟伺服器**。 **不這麼做**選取 **同盟伺服器陣列中建立第一部同盟伺服器**。  
  
如果您使用 Windows PowerShell，執行**新增\-AdfsFarmNode**。 **不這麼做**執行**安裝\-AdfsFarm**。  
  
出現提示時，提供複本 SQL Server 的主機和執行個體名稱**不**初始的 SQL server。  

---
ms.assetid: 4baefbd3-038f-44c0-85ba-f24e9722b757
title: 附錄 G-保護 Active Directory 中的系統管理員群組
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2912dfc534d751d4aa121d238dffc36c07562d76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882709"
---
# <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>附錄 g:保護 Active Directory 中的系統管理員群組

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012


## <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>附錄 g:保護 Active Directory 中的系統管理員群組  
在此情況下，使用 Enterprise Admins (EA) 和網域系統管理員 (DA) 群組，應該只在建置或災害復原案例中需要內建的系統管理員 (BA) 群組的成員資格。 中應該會有任何日常規律的使用者帳戶的內建系統管理員帳戶的網域，除了系統管理員群組中所述保護[附錄 d:保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

系統管理員是，根據預設，大部分在其各自的定義域中的 AD DS 物件的擁有者。 此群組成員資格可能必須在擁有權或取得物件的擁有權的能力是在需要的組建或災害復原案例中。 此外，DAs 和 EAs 會繼承其權限和透過其預設群組的成員資格系統管理員權限的數目。 不應該修改 Active Directory 中的特殊權限群組的巢狀的預設群組，並逐步指示，請依照下列所述，每個網域的系統管理員群組應該受到保護。  

每個樹系中的網域中的系統管理員群組：  

1.  移除所有成員在系統管理員群組，可能的例外狀況的內建的 Administrator 帳戶的網域，提供如中所述保護[附錄 d:保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

2.  在連結到包含成員伺服器和工作站，每個網域中的 Ou 的 Gpo，DA 群組應該新增到下列的使用者權限，在**電腦設定 \ 原則 \windows 設定 \ 安全性設定 \ 本機 Policies\ 使用者權限指派**:  

    -   拒絕從網路存取這台電腦  

    -   拒絕以批次工作登入  

    -   拒絕以服務方式登入  

3.  在網域控制站的樹系中每個網域中的 OU，系統管理員群組應該被授與下列使用者權限：  

    -   從網路存取這台電腦  

    -   允許本機登入  

    -   允許透過遠端桌面服務登入  

4.  稽核應該設定為傳送警示，如果對屬性或系統管理員群組的成員資格進行任何修改。  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-administrators-group"></a>從系統管理員群組移除所有成員的逐步指示  

1.  在**伺服器管理員**，按一下**工具**，然後按一下**Active Directory 使用者和電腦**。  

2.  若要移除系統管理員群組中的所有成員，請執行下列步驟：  

    1.  按兩下**系統管理員**群組，然後按一下**成員** 索引標籤。  

        ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_79.gif)  

    2.  選取群組的成員，然後按一下**移除**，按一下**是**，然後按一下**確定**。  

3.  在移除所有的系統管理員群組的成員之前，請重複步驟 2。  

#### <a name="step-by-step-instructions-to-secure-administrators-groups-in-active-directory"></a>保護 Active Directory 中的系統管理員群組的逐步指示  

1.  在 **伺服器管理員**，按一下**工具**，然後按一下**群組原則管理**。  

2.  在主控台樹狀目錄中，依序展開&lt;樹系&gt;\Domains\\&lt;網域&gt;，然後**群組原則物件**(其中&lt;樹系&gt;是樹系的名稱和&lt;網域&gt;是您要將群組原則設定的網域名稱)。  

3.  在主控台樹狀目錄中，以滑鼠右鍵按一下**群組原則物件**，然後按一下**新增**。  

    ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_80.gif)  

4.  在**新的 GPO**  對話方塊中，輸入<GPO Name>，然後按一下**確定** (其中*GPO 名稱*此 GPO 的名稱)。  

    ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_81.gif)  

5.  在 [詳細資料] 窗格中，以滑鼠右鍵按一下**<GPO Name>**，然後按一下**編輯**。  

6.  瀏覽至**電腦設定 \ 原則 \windows 設定 \ 本機原則**，然後按一下**使用者權限指派**。  

    ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_82.gif)  

7.  設定使用者權限，以防止系統管理員群組的成員透過網路存取成員伺服器和工作站，執行下列動作：  

    1.  按兩下**拒絕從網路存取這台電腦**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

    3.  型別**系統管理員**，按一下**檢查名稱**，然後按一下**確定**。  

        ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_83.gif)  

    4.  按一下  **確定**，並**確定**一次。  

8.  設定使用者權限，以防止系統管理員群組的成員無法以批次工作登入執行下列動作：  

    1.  按兩下**拒絕以批次工作登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

    3.  型別**系統管理員**，按一下**檢查名稱**，然後按一下**確定**。  

        ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_84.gif)  

    4.  按一下  **確定**，並**確定**一次。  

9. 設定使用者權限，以防止系統管理員群組的成員登入為服務執行下列動作：  

    1.  按兩下**拒絕以服務登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

    3.  型別**系統管理員**，按一下**檢查名稱**，然後按一下**確定**。  

        ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_85.gif)  

    4.  按一下  **確定**，並**確定**一次。  

10. 若要結束**群組原則管理編輯器**，按一下**檔案**，然後按一下**結束**。  

11. 在 **群組原則管理**，將 GPO 連結到成員伺服器和工作站的 Ou，執行下列動作：  

    1.  瀏覽至&lt;樹系&gt;> \Domains\\&lt;網域&gt;(其中&lt;樹系&gt;是樹系名稱並&lt;網域&gt;名稱您要將群組原則設定的網域）。  

    2.  以滑鼠右鍵按一下 OU GPO 會套用至，然後按一下**連結到現有的 GPO**。  

        ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_86.gif)  

    3.  選取您剛才建立的 GPO，然後按一下**確定**。  

        ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_87.gif)  

    4.  建立連結至包含工作站的所有其他 Ou。  

    5.  建立連結至包含成員伺服器的所有其他 Ou。  

        > [!IMPORTANT]  
        > 如果跳躍伺服器用來管理網域控制站和 Active Directory，請確定跳躍伺服器都位於此未連結 Gpo OU 中。  

        > [!NOTE]  
        > 當您實作在 Gpo 中的 Administrators 群組的限制時，Windows 會套用的設定，除了網域的系統管理員群組的電腦本機 Administrators 群組的成員。 因此，您應該小心實作限制 Administrators 群組中時。 本機登入或透過遠端桌面服務的登入，雖然系統管理員群組的成員禁止網路、 batch 和服務的登入建議只要是可行的方法是實作，並不會限制。 封鎖這些登入類型，可以封鎖合法的系統管理的電腦的本機 Administrators 群組成員。  
        >   
        > 下列螢幕擷取畫面顯示封鎖不當使用內建的組態設定本機和網域系統管理員帳戶，除了內建的本機或網域系統管理員群組的誤用。 請注意，**拒絕透過遠端桌面服務登入**使用者權限不包括系統管理員群組，因為它包含在此設定也會封鎖這些成員的本機電腦的帳戶登入系統管理員群組。 如果電腦上的服務設定為執行任何特殊權限的群組，這一節所述的內容中，實作這些設定可能導致服務和應用程式失敗。 因此，如同所有的這一節中的建議，您應該徹底測試適用性的設定您的環境中。  

        ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_88.gif)  

#### <a name="step-by-step-instructions-to-grant-user-rights-to-the-administrators-group"></a>授與使用者權限給系統管理員群組的逐步指示  

1.  在 **伺服器管理員**，按一下**工具**，然後按一下**群組原則管理**。  

2.  在主控台樹狀目錄中，依序展開<Forest>\Domains\\<Domain>，然後**群組原則物件**(其中<Forest>樹系的名稱和<Domain>是您想要的網域名稱群組原則設定）。  

3.  在主控台樹狀目錄中，以滑鼠右鍵按一下**群組原則物件**，然後按一下**新增**。  

    ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_89.gif)  

4.  在**新的 GPO**  對話方塊中，輸入<GPO Name>，然後按一下**確定** (其中<GPO Name>此 GPO 的名稱)。  

    ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_90.gif)  

5.  在 [詳細資料] 窗格中，以滑鼠右鍵按一下**<GPO Name>**，然後按一下**編輯**。  

6.  瀏覽至**電腦設定 \ 原則 \windows 設定 \ 本機原則**，然後按一下**使用者權限指派**。  

    ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_91.gif)  

7.  設定使用者的權限允許透過網路存取網域控制站的系統管理員群組的成員，執行下列動作：  

    1.  按兩下**從網路存取這台電腦**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

    3.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

        ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_92.gif)  

    4.  按一下  **確定**，並**確定**一次。  

8.  設定要允許 Administrators 群組的成員，透過下列方式在本機登入的使用者權限：  

    1.  按兩下**允許本機登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

    3.  型別**系統管理員**，按一下核取**名稱**，然後按一下**確定**。  

        ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_93.gif)  

    4.  按一下  **確定**，並**確定**一次。  

9. 設定使用者的權限讓系統管理員群組的成員，透過遠端桌面服務登入執行下列動作：  

    1.  按兩下**允許透過遠端桌面服務登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

    3.  型別**系統管理員**，按一下**檢查名稱**，然後按一下**確定**。  

        ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_94.gif)  

    4.  按一下  **確定**，並**確定**一次。  

10. 若要結束**群組原則管理編輯器**，按一下**檔案**，然後按一下**結束**。  

11. 在 **群組原則管理**，將 GPO 連結到網域控制站 OU，執行下列動作：  

    1.  瀏覽至<Forest>\Domains\\ <Domain> (其中<Forest>樹系的名稱和<Domain>是您要將群組原則設定的網域名稱)。  

    2.  以滑鼠右鍵按一下 OU，然後按一下 網域控制站**連結到現有的 GPO**。  

        ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_95.gif)  

    3.  選取您剛才建立的 GPO，然後按一下**確定**。  

        ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_96.gif)  

#### <a name="verification-steps"></a>驗證步驟  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>確認 「 拒絕從網路存取這台電腦 」 GPO 設定  
從任何成員伺服器或工作站 GPO 變更 （例如 「 跳躍伺服器 」） 不會受到影響，嘗試存取的成員伺服器或工作站 GPO 變更的影響網路上。 若要確認 GPO 設定，請嘗試使用對應的系統磁碟機**NET USE**命令。  

1.  在本機使用屬於 Administrators 群組的成員帳戶登入。  

2.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

3.  在**搜尋**方塊中，輸入**命令提示字元**，以滑鼠右鍵按一下**命令提示字元**，然後按一下**系統管理員身分執行**開啟已提升權限命令提示字元。  

4.  當系統提示您核准提升權限，按一下**是**。  

    ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_97.gif)  

5.  在 **命令提示字元**視窗中，輸入**net 使用\\ \\\<伺服器名稱\>\c$**，其中\<伺服器名稱\>是成員伺服器或您嘗試透過網路存取的工作站的名稱。  

6.  下列螢幕擷取畫面會顯示應該會出現錯誤訊息。  

    ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_98.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認 「 拒絕登入以批次工作 」 GPO 設定  
從任何成員伺服器或工作站 GPO 變更的影響，在本機登入。  

###### <a name="create-a-batch-file"></a>建立批次檔  

1.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

2.  中**搜尋**方塊中，輸入**記事本**，然後按一下**記事本**。  

3.  在 **記事本**，型別**dir c:**。  

4.  按一下 **檔案**，然後按一下**另存新檔**。  

5.  在 **檔名**欄位中，輸入 **<Filename>.bat** (其中<Filename>是新的批次檔的名稱)。  

###### <a name="schedule-a-task"></a>排程的工作  

1.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

2.  在 **搜尋**方塊中，輸入**工作排程器**，然後按一下**工作排程器**。  

    > [!NOTE]  
    > 在 [搜尋] 方塊中，執行 Windows 8 的電腦上輸入排程工作，然後按一下排程工作。  

3.  按一下 **動作**，然後按一下**建立工作**。  

4.  在 [**建立工作**] 對話方塊中，輸入**<Task Name>** (其中<Task Name>是新工作的名稱)。  

5.  按一下 **動作**索引標籤，然後按一下**新增**。  

6.  在 **動作**欄位中，選取**啟動程式**。  

7.  在 **程式或指令碼**欄位中，按一下**瀏覽**，找出並選取 批次檔中建立**建立批次檔**區段，然後按一下 **開啟**.  

8.  按一下 [確定] 。  

9. 按一下 [一般] 索引標籤。  

10. 在 **安全性選項**欄位中，按一下**變更使用者或群組**。  

11. 輸入系統管理員群組的成員帳戶的名稱，再按一下**檢查名稱**，然後按一下 **[確定]**。  

12. 選取 **執行是否有使用者未登入的 onor**並**不要儲存密碼**。 工作將只包含本機電腦資源的存取權。  

13. 按一下 [確定] 。  

14. 應該會出現一個對話方塊，要求的使用者帳戶的認證來執行工作。  

15. 輸入密碼之後，按一下**確定**。  

16. 應該會出現如下所示的對話方塊。  

    ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_99.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認 「 拒絕登入為服務 」 GPO 設定  

1.  從任何成員伺服器或工作站 GPO 變更的影響，在本機登入。  

2.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

3.  在 **搜尋**方塊中，輸入**服務**，然後按一下**服務**。  

4.  找出並按兩下**列印多工緩衝處理器**。  

5.  单击 **“登录”** 选项卡。  

6.  在 **身分登入**欄位中，選取**此帳戶**。  

7.  按一下 **瀏覽**，輸入屬於 Administrators 群組的成員帳戶的名稱，按一下**檢查名稱**，然後按一下**確定**。  

8.  在 **密碼**並**確認密碼**欄位中，輸入所選的帳戶的密碼，然後按一下**確定**。  

9. 按一下 **確定**三次。  

10. 以滑鼠右鍵按一下**列印多工緩衝處理器**然後按一下**重新啟動**。  

11. 重新啟動服務時，應該會出現如下所示的對話方塊。  

    ![安全的系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_100.png)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>還原的列印多工緩衝處理器服務的變更  

1.  從任何成員伺服器或工作站 GPO 變更的影響，在本機登入。  

2.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

3.  在 **搜尋**方塊中，輸入**服務**，然後按一下**服務**。  

4.  找出並按兩下**列印多工緩衝處理器**。  

5.  单击 **“登录”** 选项卡。  

6.  在 **身分登入**欄位中，按一下 **本機系統**帳戶，再按一下**確定**。  

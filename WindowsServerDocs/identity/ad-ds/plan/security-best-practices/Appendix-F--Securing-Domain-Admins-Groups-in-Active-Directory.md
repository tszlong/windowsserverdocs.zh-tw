---
ms.assetid: 017b88a6-f29b-4787-99b6-b5c8eaf8c3df
title: "附錄 F-保護在 Active Directory Domain 管理員群組"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3535714e0df43cb94c7e89c503bc5f875cd49e28
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>在 Active Directory 中附錄 f︰ 保護網域管理員群組

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>在 Active Directory 中附錄 f︰ 保護網域管理員群組  
企業系統管理員 (EA) 群組一樣，應該只在組建或損壞修復案例中需要網域系統管理員 (DA) 群組成員資格。 應該會不日常帳號建網域中，除了 DA 群組中所述保護[附錄 d 保護建系統管理員帳號 Active Directory 在](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

網域系統管理員是，預設的所有成員伺服器與各自網域中的工作站本機系統管理員群組成員。 此預設巢不應修改性和損壞復原。 如果您已從本機系統管理員群組成員伺服器移除網域系統管理員 」，應該新增到每個成員 server 和工作站網域中的系統管理員群組。 每個網域的網域管理群組逐步指示，請依照下列中所述安全。  

森林中的每個網域中的網域管理員群組：  

1.  移除所有成員群組中，可能的建網域中，除了所述的保護提供[附錄 d 保護建系統管理員帳號 Active Directory 在](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

2.  Gpo 連結到 Ou 包含成員伺服器及工作站每個網域中的，DA 群組應該新增到使用者權限在下列**電腦設定 \ 原則 \windows 安全性設定本機 Settings\User 權限指派**:  

    -   拒絕從網路存取此電腦  

    -   拒絕以分批登入  

    -   拒絕登入即服務  

    -   在本機拒絕登入  

    -   透過遠端桌面服務使用者權利拒絕登入  

3.  稽核應該會傳送任何修改網域管理群組成員資格或屬性警示設定。  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-domain-admins-group"></a>適用於所有成員都移除網域管理員群組逐步指示  

1.  在**伺服器管理員**，按一下 [**工具**，並按一下 [ **Active Directory 使用者和電腦**。  

2.  若要移除的 DA 群組的所有成員，執行下列步驟：  

    1.  按兩下**網域系統管理員**群組中，按一下 [**成員**索引標籤。  

        ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_62.gif)  

    2.  選取的群組成員，請按一下**移除**，按一下 [**是**，並按一下 [ **[確定]**。  

3.  已經移除 DA 群組的所有成員重複步驟 2。  

#### <a name="step-by-step-instructions-to-secure-domain-admins-in-active-directory"></a>逐步指示安全在 Active Directory Domain 系統管理員  

1.  在**伺服器管理員**，按一下 [**工具**，並按**群組原則管理**。  

2.  在主控台中，展開 \ < Forest\ > \\Domains\\\ < Domain\ >，然後**群組原則物件**(其中 \ < Forest\ > 是樹系的名稱及 \ < Domain\ > 是您想要設定群組原則設定的網域名稱)。  

3.  在主機上按一下滑鼠右鍵**群組原則物件**，按一下 [**新增]**。  

    ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_63.gif)  

4.  在**新的 GPO**對話方塊中，輸入 \ < GPO Name\ >，按**[確定]** (位置 \ < GPO Name\ > 是此 GPO 的名稱)。  

    ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_64.gif)  

5.  在詳細資料窗格中，以滑鼠右鍵按一下 \ < GPO Name\ >，然後按一下**編輯**。  

6.  瀏覽至**電腦設定 \ 原則 \windows 安全性設定本機原則**，按一下 [**權限指派使用者]**。  

    ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_65.gif)  

7.  設定使用者權限以避免網域管理群組成員存取伺服器成員和工作站在網路上執行下列：  

    1.  按兩下**拒絕從網路存取這台電腦**，然後選取**定義這些原則設定**。  

    2.  按一下**[新增使用者或群組**，按一下 [**瀏覽]**。  

    3.  輸入**網域系統管理員**，按一下 [**檢查名稱**，並按一下 [ **[確定]**。  

        ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_66.gif)  

    4.  按一下**[確定]**，以及**[確定]**一次。  

8.  設定使用者權限以避免 DA 群組成員分批身分登入，方法如下：  

    1.  按兩下**拒絕以分批登入**，然後選取**定義這些原則設定**。  

    2.  按一下**[新增使用者或群組**，按一下 [**瀏覽]**。  

    3.  輸入**網域系統管理員**，按一下 [**檢查名稱**，並按一下 [ **[確定]**。  

        ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_67.gif)  

    4.  按一下**[確定]**，以及**[確定]**一次。  

9. 設定使用者權限防止 DA 群組成員登入以服務，方法如下：  

    1.  按兩下**以服務拒絕登入**，然後選取**定義這些原則設定**。  

    2.  按一下**[新增使用者或群組**，按一下 [**瀏覽]**。  

    3.  輸入**網域系統管理員**，按一下 [**檢查名稱**，並按一下 [ **[確定]**。  

        ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_68.gif)  

    4.  按一下**[確定]**，以及**[確定]**一次。  

10. 設定使用者權限以避免網域管理群組成員登入本機成員伺服器以及工作站，方法如下：  

    1.  按兩下**在本機拒絕登入**，然後選取**定義這些原則設定**。  

    2.  按一下**[新增使用者或群組**，按一下 [**瀏覽]**。  

    3.  輸入**網域系統管理員**，按一下 [**檢查名稱**，並按一下 [ **[確定]**。  

        ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_69.gif)  

    4.  按一下**[確定]**，以及**[確定]**一次。  

11. 設定使用者權限以避免網域管理群組成員存取成員伺服器，並透過遠端桌面服務工作站，方法如下：  

    1.  按兩下**透過遠端桌面服務拒絕登入**，然後選取**定義這些原則設定**。  

    2.  按一下**[新增使用者或群組**，按一下 [**瀏覽]**。  

    3.  輸入**網域系統管理員**，按一下 [**檢查名稱**，並按一下 [ **[確定]**。  

        ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_70.gif)  

    4.  按一下**[確定]**，以及**[確定]**一次。  

12. 結束**群組原則編輯器] 管理**，按一下 [**檔案**，並按**結束**。  

13. 在群組原則管理，將 GPO 連結到工作站 Ou 與成員伺服器，方法如下：  

    1.  瀏覽 \ < Forest\ > \Domains\\\ < Domain\ > (位置 \ < Forest\ > 是樹系的名稱及 \ < Domain\ > 是您想要設定群組原則設定的網域名稱)。  

    2.  以滑鼠右鍵按一下組織單位，將會套用至 GPO，然後按一下**的現有 GPO 連結**。  

        ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_71.gif)  

    3.  選取您剛建立 GPO 並按一下 [ **[確定]**。  

        ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_72.gif)  

    4.  建立包含工作站所有其他 Ou 的連結。  

    5.  建立所有其他 Ou 包含成員伺服器的連結。  

        > [!IMPORTANT]  
        > 如果捷徑伺服器可用來管理網域控制站和 Active Directory，確定捷徑伺服器位於組織單位此 Gpo 不連結。  

#### <a name="verification-steps"></a>步驟驗證  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>請檢查 「 Deny 從網路存取此電腦] GPO 設定  
從任何成員伺服器或 GPO 變更 （例如 「 捷徑伺服器） 」 不會受到影響的工作站，嘗試透過受 GPO 變更網路存取成員伺服器或工作站。 要檢查 GPO 設定，請嘗試將系統磁碟機對應使用**網路使用**命令。  

1.  登入本機使用 account 網域管理群組成員。  

2.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

3.  在**搜尋**方塊中，輸入**命令提示字元**，以滑鼠右鍵按一下**命令提示字元**，，然後按一下**以系統管理員身分執行**打開提升權限的命令提示字元。  

4.  核准提高權限提示，請按一下**[是]**。  

    ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_73.gif)  

5.  在**命令提示字元**視窗中，輸入**網路使用 \\\ \c$ < 伺服器 Name\ >**，其中 \ < 伺服器 Name\ > 是您嘗試在網路上存取的工作站成員伺服器的名稱。  

6.  下列螢幕擷取畫面顯示應該會出現錯誤訊息。  

    ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_74.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認 [拒絕登入分批為 「 GPO 設定  

從任何成員伺服器或受到 GPO 變更工作站，登入本機。  

###### <a name="create-a-batch-file"></a>建立批次檔案  

1.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

2.  在**搜尋**方塊中，輸入**「 記事本 」**，並按**記事本**。  

3.  在**[記事本]**，輸入**dir c:**。  

4.  按一下**檔案**，按一下 [**儲存為**。  

5.  在**檔案**名稱] 欄位中，輸入**\.bat < Filename\ >** (位置 \ < Filename\ > 是新的 「 批次檔案的名稱)。  

###### <a name="schedule-a-task"></a>排程工作  

1.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

2.  在**搜尋**方塊中，輸入**工作排程器**，並按**工作排程器**。  

    > [!NOTE]  
    > 在電腦上執行 Windows 8 的**搜尋**方塊中，輸入**排程工作**，並按**排程工作**。  

3.  在**工作排程器**功能表列中，按**動作**，並按一下 [**建立工作**。  

4.  在**建立工作**對話方塊中，輸入**\ < 工作 Name\ >** (位置 \ < 工作 Name\ > 是新工作的名稱)。  

5.  按一下**動作**索引標籤，然後按**新增]**。  

6.  在**動作**欄位中，選取**開始程式]**。  

7.  在**程式日指令碼**，按一下 [**瀏覽**，找出並選取 [建立在 「 批次檔案**建立批次檔案**區段，然後按一下**開放**。  

8.  按一下**[確定]**。  

9. 按一下**一般**索引標籤。  

10. 在**安全性**按一下 [選項]**變更使用者或群組**。  

11. 輸入網域管理群組成員 account 的名稱，請按一下**檢查名稱]**，按一下 [ **[確定]**。  

12. 選取 [**是否使用者登入或不執行**，然後選取**不要儲存密碼**。 任務將只可以存取本機電腦資源。  

13. 按一下**[確定]**。  

14. 應該會出現一個對話方塊，要求帳號認證執行的工作。  

15. 輸入認證之後, 請按**[確定]**。  

16. 應該會出現一個對話方塊類似下列。  

    ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_75.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認 [拒絕登入即服務 」 GPO 設定  

1.  從任何成員伺服器或受到 GPO 變更工作站，登入本機。  

2.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

3.  在**搜尋**方塊中，輸入**服務**，並按**服務**。  

4.  找出並按兩下 [**列印多工緩衝處理器**。  

5.  按一下**登入**索引標籤。  

6.  在**以登入**，請選取**此 account**選項。  

7.  按一下**瀏覽**，輸入名稱為網域管理群組成員後，按**檢查名稱**，並按一下 [ **[確定]**。  

8.  在**密碼**和**確認密碼**、 輸入選取的 account 的密碼，然後按一下 [ **[確定]**。  

9. 按一下**[確定]**三次。  

10. 以滑鼠右鍵按一下**列印多工緩衝處理器**，按一下 [**重新開機**。  

11. 服務會重新開始時，應該會顯示對話方塊中，如下所示。  

    ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_76.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>還原已變更的印表機多工緩衝處理器服務  

1.  從任何成員伺服器或受到 GPO 變更工作站，登入本機。  

2.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

3.  在**搜尋**方塊中，輸入**服務**，並按**服務**。  

4.  找出並按兩下 [**列印多工緩衝處理器**。  

5.  按一下**登入**索引標籤。  

6.  在**以登入**，請選取**本機系統**帳號，並按**[確定]**。  

##### <a name="verify-deny-log-on-locally-gpo-settings"></a>確認 [拒絕登入本機 」 GPO 設定  

1.  從任何成員伺服器或工作站受到 GPO 變更，嘗試登入本機使用 account 網域管理群組成員。 應該會出現一個對話方塊類似下列。  

    ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_77.gif)  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>確認 [拒絕登入遠端桌面服務透過 「 GPO 設定    
1.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

2.  在**搜尋**方塊中，輸入**遠端桌面連接**，並按**遠端桌面連接**。  

3.  在**電腦**欄位中，輸入您想要連接，然後按一下電腦名稱**連接**。 （您也可以輸入 IP 位址，而不是電腦名稱）。  

4.  出現提示時，提供的認證帳號網域管理群組成員。  

5.  應該會出現一個對話方塊類似下列。  

    ![安全網域管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_78.gif)  

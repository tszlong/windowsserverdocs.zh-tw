---
ms.assetid: 4baefbd3-038f-44c0-85ba-f24e9722b757
title: "附錄 G-保護 Active Directory 中的系統管理員群組"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4cf89f7bf31ce848de384778b0213d0ddc822158
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>在 Active Directory 中附錄 g：保護系統管理員群組

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>在 Active Directory 中附錄 g：保護系統管理員群組  
企業系統管理員 (EA) 和網域系統管理員 (DA) 群組一樣，應該只在組建或損壞修復案例中需要建系統管理員 (BA) 群組成員資格。 應該有不日常帳號系統管理員除外網域建系統管理員負責群組中所述保護[附錄 d 保護建系統管理員帳號 Active Directory 在](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

系統管理員是，大部分各自網域中 AD DS 物件的擁有者預設。 此群組成員資格可能需要在組建或損壞擁有權或拍攝物件的擁有權的功能是在需要復原案例中。 此外，DAs 和 EAs 繼承他們的權限和一定他們預設群組中的成員系統管理員權限的數字。 不應修改巢 Active Directory 中有特殊權限的群組預設群組，與每個網域中的系統管理員群組逐步指示，請依照下列中所述安全。  

森林中的每個網域中的系統管理員群組：  

1.  移除所有成員的系統管理員群組中，可能的建網域中，除了所述的保護提供[附錄 d 保護建系統管理員帳號 Active Directory 在](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

2.  Gpo 連結到 Ou 包含成員伺服器及工作站每個網域中的，DA 群組應該新增到使用者權限在下列**電腦設定 \ 原則 \windows 安全性設定本機 Policies\ 使用者權限指派**:  

    -   拒絕從網路存取此電腦  

    -   拒絕以分批登入  

    -   拒絕登入即服務  

3.  在網域控制站在森林中的每個網域中，系統管理員群組應被授與下列使用者權限：  

    -   從網路存取此電腦  

    -   在本機允許登入  

    -   允許登入透過遠端桌面服務  

4.  稽核應該設定的系統管理員群組成員資格或屬性任何修改傳送通知。  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-administrators-group"></a>移除所有成員從系統管理員群組逐步指示  

1.  在**伺服器管理員**，按一下 [**工具**，並按一下 [ **Active Directory 使用者和電腦**。  

2.  若要從系統管理員群組中移除所有的成員，執行下列步驟：  

    1.  按兩下**系統管理員**群組中，按一下 [**成員**索引標籤。  

        ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_79.gif)  

    2.  選取的群組成員，請按一下**移除**，按一下 [**是**，並按一下 [ **[確定]**。  

3.  系統管理員群組的所有成員都移除了重複步驟 2。  

#### <a name="step-by-step-instructions-to-secure-administrators-groups-in-active-directory"></a>安全 Active Directory 中的系統管理員群組逐步指示  

1.  在**伺服器管理員**，按一下 [**工具**，並按**群組原則管理**。  

2.  在主控台中，展開<Forest>\Domains\\<Domain>，然後**群組原則物件**(其中<Forest>樹系的名稱和<Domain>是您想要設定群組原則設定的網域名稱)。  

3.  在主機上按一下滑鼠右鍵**群組原則物件**，按一下 [**新增]**。  

    ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_80.gif)  

4.  在**新的 GPO**對話方塊中，輸入<GPO Name>，按一下**[確定]** (其中*GPO 的名稱*是此 GPO 的名稱)。  

    ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_81.gif)  

5.  在詳細資料窗格中，以滑鼠右鍵按一下** <GPO Name> **，然後按一下**編輯**。  

6.  瀏覽至**電腦設定 \ 原則 \windows 安全性設定本機原則**，按一下 [**權限指派使用者]**。  

    ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_82.gif)  

7.  設定使用者權限以避免的系統管理員群組成員存取伺服器成員和工作站在網路上執行下列：  

    1.  按兩下**拒絕從網路存取這台電腦**，然後選取**定義這些原則設定**。  

    2.  按一下**[新增使用者或群組**，按一下 [**瀏覽]**。  

    3.  輸入**系統管理員**，按一下 [**檢查名稱]**，並按一下 [ **[確定]**。  

        ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_83.gif)  

    4.  按一下**[確定]**，以及**[確定]**一次。  

8.  設定使用者權限以防止的系統管理員群組成員分批身分登入，方法如下：  

    1.  按兩下**拒絕以分批登入**，然後選取**定義這些原則設定**。  

    2.  按一下**[新增使用者或群組**，按一下 [**瀏覽]**。  

    3.  輸入**系統管理員**，按一下 [**檢查名稱]**，並按一下 [ **[確定]**。  

        ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_84.gif)  

    4.  按一下**[確定]**，以及**[確定]**一次。  

9. 設定使用者防止的系統管理員群組成員執行以下動作來登入以服務的權限：  

    1.  按兩下**以服務拒絕登入**，然後選取**定義這些原則設定**。  

    2.  按一下**[新增使用者或群組**，按一下 [**瀏覽]**。  

    3.  輸入**系統管理員**，按一下 [**檢查名稱]**，並按一下 [ **[確定]**。  

        ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_85.gif)  

    4.  按一下**[確定]**，以及**[確定]**一次。  

10. 結束**群組原則編輯器] 管理**，按一下 [**檔案**，並按**結束**。  

11. 在**群組原則管理**，將 GPO 連結到工作站 Ou 與成員伺服器，方法如下：  

    1.  瀏覽至<Forest>\Domains\\<Domain> (其中<Forest>是樹系的名稱及<Domain>是您想要設定群組原則設定的網域名稱)。  

    2.  以滑鼠右鍵按一下組織單位，將會套用至 GPO，然後按一下**的現有 GPO 連結**。  

        ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_86.gif)  

    3.  選取您剛建立 GPO 並按一下 [ **[確定]**。  

        ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_87.gif)  

    4.  建立包含工作站所有其他 Ou 的連結。  

    5.  建立所有其他 Ou 包含成員伺服器的連結。  

        > [!IMPORTANT]  
        > 如果捷徑伺服器可用來管理網域控制站和 Active Directory，確定捷徑伺服器位於組織單位此 Gpo 不連結。  

        > [!NOTE]  
        > 當您的系統管理員群組 Gpo 上實作限制時，Windows 會套用的設定，除了網域中的系統管理員群組的電腦本機系統管理員群組成員。 因此，您應該時小心限制實作系統管理員 」 群組。 網路、 批次，以及服務登入禁止系統管理員群組成員建議不論是可行實作，但不會限制登入本機或透過遠端桌面服務登入。 封鎖這些登入類型，可以封鎖合法管理某部電腦的系統管理員本機群組成員。  
        >   
        > 下列螢幕擷取畫面顯示本機封鎖不當建設定和網域系統管理員帳號，除了不當建本機或網域系統管理員 」 群組。 請注意，**透過遠端桌面服務拒絕登入**使用者權限，不包含管理員群組，因為包含在此設定也會封鎖這些登入，必須在本機電腦的系統管理員群組成員。 電腦上的服務執行操作有特殊權限的群組本節所述的設定，如果實作這些設定可能會造成服務和應用程式失敗。 因此，與的所有建議在本區段中，您應該會完全都測試適用於設定您的環境中。  

        ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_88.gif)  

#### <a name="step-by-step-instructions-to-grant-user-rights-to-the-administrators-group"></a>權限授與使用者系統管理員群組逐步指示  

1.  在**伺服器管理員**，按一下 [**工具**，並按**群組原則管理**。  

2.  在主控台中，展開<Forest>\Domains\\<Domain>，然後**群組原則物件**(其中<Forest>樹系的名稱和<Domain>是您想要設定群組原則設定的網域名稱)。  

3.  在主機上按一下滑鼠右鍵**群組原則物件**，按一下 [**新增]**。  

    ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_89.gif)  

4.  在**新的 GPO**對話方塊中，輸入<GPO Name>，按一下**[確定]** (其中<GPO Name>是此 GPO 的名稱)。  

    ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_90.gif)  

5.  在詳細資料窗格中，以滑鼠右鍵按一下** <GPO Name> **，然後按一下**編輯**。  

6.  瀏覽至**電腦設定 \ 原則 \windows 安全性設定本機原則**，按一下 [**權限指派使用者]**。  

    ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_91.gif)  

7.  設定使用者權限允許網路上的存取網域控制站的系統管理員群組成員，方法如下：  

    1.  按兩下**到這部電腦從網路存取**，然後選取**定義這些原則設定**。  

    2.  按一下**[新增使用者或群組**，按一下 [**瀏覽]**。  

    3.  按一下**[新增使用者或群組**，按一下 [**瀏覽]**。  

        ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_92.gif)  

    4.  按一下**[確定]**，以及**[確定]**一次。  

8.  設定使用者權限以讓系統管理員群組成員登入本機，方法如下：  

    1.  按兩下**在本機允許登入**，然後選取**定義這些原則設定**。  

    2.  按一下**[新增使用者或群組**，按一下 [**瀏覽]**。  

    3.  輸入**系統管理員**，按一下 [檢查]**名稱**，並按一下 [ **[確定]**。  

        ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_93.gif)  

    4.  按一下**[確定]**，以及**[確定]**一次。  

9. 設定使用者權限，好讓系統管理員群組成員執行以下動作來登入透過遠端桌面服務：  

    1.  按兩下**允許透過遠端桌面服務登入**，然後選取**定義這些原則設定**。  

    2.  按一下**[新增使用者或群組**，按一下 [**瀏覽]**。  

    3.  輸入**系統管理員**，按一下 [**檢查名稱]**，並按一下 [ **[確定]**。  

        ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_94.gif)  

    4.  按一下**[確定]**，以及**[確定]**一次。  

10. 結束**群組原則編輯器] 管理**，按一下 [**檔案**，並按**結束**。  

11. 在**群組原則管理**，將 GPO 連結到網域控制站，方法如下：  

    1.  瀏覽至<Forest>\Domains\\<Domain> (其中<Forest>是樹系的名稱及<Domain>是您想要設定群組原則設定的網域名稱)。  

    2.  以滑鼠右鍵按一下網域控制站組織單位，然後按一下**的現有 GPO 連結**。  

        ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_95.gif)  

    3.  選取您剛建立 GPO 並按一下 [ **[確定]**。  

        ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_96.gif)  

#### <a name="verification-steps"></a>步驟驗證  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>請檢查 「 Deny 從網路存取此電腦] GPO 設定  
從任何成員伺服器或 GPO 變更 （例如 「 捷徑伺服器） 」 不會受到影響的工作站，嘗試透過受 GPO 變更網路存取成員伺服器或工作站。 要檢查 GPO 設定，請嘗試將系統磁碟機對應使用**網路使用**命令。  

1.  登入本機使用為系統管理員群組成員。  

2.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

3.  在**搜尋**方塊中，輸入**命令提示字元**，以滑鼠右鍵按一下**命令提示字元**，，然後按一下**以系統管理員身分執行**打開提升權限的命令提示字元。  

4.  核准提高權限提示，請按一下**[是]**。  

    ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_97.gif)  

5.  在**命令提示字元**視窗中，輸入**網路使用 \\\<Server Name>\c$**，其中<Server Name>是您嘗試在網路上存取的工作站成員伺服器的名稱。  

6.  下列螢幕擷取畫面顯示應該會出現錯誤訊息。  

    ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_98.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認 [拒絕登入分批為 「 GPO 設定  
從任何成員伺服器或受到 GPO 變更工作站，登入本機。  

###### <a name="create-a-batch-file"></a>建立批次檔案  

1.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

2.  在**搜尋**方塊中，輸入**「 記事本 」**，並按**記事本**。  

3.  在**[記事本]**，輸入**dir c:**。  

4.  按一下**檔案**，按一下 [**儲存為**。  

5.  在**檔案名稱**欄位中，輸入** <Filename>.bat** (其中<Filename>是新的 「 批次檔案的名稱)。  

###### <a name="schedule-a-task"></a>排程工作  

1.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

2.  在**搜尋**方塊中，輸入**工作排程器**，並按**工作排程器**。  

    > [!NOTE]  
    > 在搜尋方塊中，執行 Windows 8，電腦上輸入排程工作，並按一下 [排程工作。  

3.  按一下**動作**，按一下 [**建立工作**。  

4.  在**建立工作**對話方塊中，輸入** <Task Name> ** (其中<Task Name>是新工作的名稱)。  

5.  按一下**動作**索引標籤，然後按**新增]**。  

6.  在**動作**欄位中，選取**開始程式]**。  

7.  在**程式日指令碼**欄位中，按一下 [**瀏覽]**，找出並選取 [建立在 「 批次檔案**建立批次檔案**區段，然後按一下**開放**。  

8.  按一下**[確定]**。  

9. 按一下**一般**索引標籤。  

10. 在**安全性選項**欄位中，按**變更使用者或群組**。  

11. 輸入系統管理員群組成員 account 的名稱，請按一下**檢查名稱]**，按一下 [ **[確定]**。  

12. 選取 [**使用者是否已登的 onor 不執行**和**不要儲存密碼**。 任務將只可以存取本機電腦資源。  

13. 按一下**[確定]**。  

14. 應該會出現一個對話方塊，要求帳號認證執行的工作。  

15. 之後輸入密碼，請按**[確定]**。  

16. 應該會出現一個對話方塊類似下列。  

    ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_99.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認 [拒絕登入即服務 」 GPO 設定  

1.  從任何成員伺服器或受到 GPO 變更工作站，登入本機。  

2.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

3.  在**搜尋**方塊中，輸入**服務**，並按**服務**。  

4.  找出並按兩下 [**列印多工緩衝處理器**。  

5.  按一下**登入**索引標籤。  

6.  在**以**欄位中，選取**此 account**。  

7.  按一下**瀏覽**，輸入名稱為系統管理員群組成員後，按**檢查名稱**，並按一下 [ **[確定]**。  

8.  在**密碼**和**確認密碼**欄位，輸入所選取的密碼，然後按一下 [ **[確定]**。  

9. 按一下**[確定]**三次。  

10. 以滑鼠右鍵按一下**列印多工緩衝處理器**，按一下 [**重新開機**。  

11. 服務會重新開始時，應該會顯示對話方塊中，如下所示。  

    ![安全管理員群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_100.png)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>還原已變更的印表機多工緩衝處理器服務  

1.  從任何成員伺服器或受到 GPO 變更工作站，登入本機。  

2.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

3.  在**搜尋**方塊中，輸入**服務**，並按**服務**。  

4.  找出並按兩下 [**列印多工緩衝處理器**。  

5.  按一下**登入**索引標籤。  

6.  在**以登入**欄位中，按一下 [**本機系統**帳號，並按**[確定]**。  

---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: "附錄 H-保護本機系統管理員帳號，並群組"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 222e6725456bb76267240467469e97c5b64fc888
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>附錄 H:WINDOWS 保護本機系統管理員帳號，並群組

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>附錄 H:WINDOWS 保護本機系統管理員帳號，並群組  
在所有 Windows 版本中目前的主要支援，本機停用根據預設，這會讓 account pass hash 和其他認證竊取攻擊無法使用。 但是，在環境中所包含的舊版作業系統或中有已經支援本機系統管理員帳號，這些帳號可成員伺服器和工作站傳播危害上文所述。 每個本機系統管理員 account 及群組應該保護逐步指示，請依照下列中所述。  

考量保護建系統管理員 (BA) 群組中的相關詳細資訊，請查看[實作最低權限管理型號](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)。  

#### <a name="controls-for-local-administrator-accounts"></a>本機系統管理員帳號控制  
針對每一個網域中的樹系本機系統管理員帳號，您應該進行下列設定：  

-   設定限制的系統管理員核對的使用加入網域的系統 Gpo  
    -   在您建立和連結工作站和成員伺服器 Ou 每個網域中的一或多個 Gpo，將管理員新增至下列使用者權限在**電腦設定 \ 原則 \windows 安全性設定本機 Settings\User 權限指派**:  

        -   拒絕從網路存取此電腦  

        -   拒絕以分批登入  

        -   拒絕登入即服務  

        -   透過遠端桌面服務拒絕登入  

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>安全本機系統管理員群組逐步指示  

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>設定限制加入網域的系統管理員 Gpo  

1.  在**伺服器管理員**，按一下 [**工具**，並按**群組原則管理**。  

2.  在主控台中，展開<Forest>\Domains\\<Domain>，然後**群組原則物件**(其中<Forest>樹系的名稱和<Domain>是您想要設定群組原則設定的網域名稱)。  

3.  在主機上按一下滑鼠右鍵**群組原則物件**，按一下 [**新增]**。  

    ![安全本機系統管理員帳號，並群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)  

4.  在**新的 GPO**對話方塊中，輸入** <GPO Name> **，然後按一下**[確定]** (位置<GPO Name>此 GPO 的名稱)。  

    ![安全本機系統管理員帳號，並群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)  

5.  在詳細資料窗格中，以滑鼠右鍵按一下** <GPO Name> **，然後按一下**編輯**。  

6.  瀏覽至**電腦設定 \ 原則 \windows 安全性設定本機原則**，按一下 [**權限指派使用者]**。  

    ![安全本機系統管理員帳號，並群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)  

7.  設定使用者權限以避免本機伺服器成員和工作站網路存取，方法如下：  

    1.  按兩下**拒絕從網路存取這台電腦**，然後選取**定義這些原則設定**。  

    2.  按一下**新增使用者或群組**，輸入本機的使用者名稱，然後按**[確定]**。 此使用者名稱將會**系統管理員**時已安裝 Windows,，預設值。  

        ![安全本機系統管理員帳號，並群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)  

    3.  按一下 \ [確定 \]。  

        > [!IMPORTANT]  
        > 當您新增這些設定的系統管理員帳號時，您可以指定是否您所設定的本機或核對系統管理員的帳號的標籤。 例如，若要新增這些 TAILSPINTOYS 網域中的系統管理員 account 拒絕權限，您可以瀏覽以系統管理員負責 TAILSPINTOYS 網域中，它會顯示為 TAILSPINTOYS\Administrator。 如果您輸入**系統管理員**中的這些使用者權限設定群組原則物件編輯器] 中，您將會限制 GPO 所套用的每一部電腦上本機系統管理員 account 之前所述。  

8.  設定使用者權限以避免本機分批身分登入，方法如下：  

    1.  按兩下**拒絕以分批登入**，然後選取**定義這些原則設定**。  

    2.  按一下**新增使用者或群組**，輸入本機的使用者名稱，然後按**[確定]**。 此使用者名稱將會**系統管理員**時已安裝 Windows,，預設值。  

        ![安全本機系統管理員帳號，並群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)  

    3.  按一下**[確定]**。  

        > [!IMPORTANT]  
        > 當您新增這些設定的系統管理員帳號時，您可以指定是否您所設定的本機或核對系統管理員的帳號的標籤。 例如，若要新增這些 TAILSPINTOYS 網域中的系統管理員 account 拒絕權限，您可以瀏覽以系統管理員負責 TAILSPINTOYS 網域中，它會顯示為 TAILSPINTOYS\Administrator。 如果您輸入**系統管理員**中的這些使用者權限設定群組原則物件編輯器] 中，您將會限制 GPO 所套用的每一部電腦上本機系統管理員 account 之前所述。  

9. 設定使用者防止本機執行以下動作來登入以服務的權限：  

    1.  按兩下**以服務拒絕登入**，然後選取**定義這些原則設定**。  

    2.  按一下**新增使用者或群組**，輸入本機的使用者名稱，然後按**[確定]**。 此使用者名稱將會**系統管理員**時已安裝 Windows,，預設值。  

        ![安全本機系統管理員帳號，並群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)  

    3.  按一下**[確定]**。  

        > [!IMPORTANT]  
        > 當您新增這些設定的系統管理員帳號時，您可以指定是否您所設定的本機或核對系統管理員的帳號的標籤。 例如，若要新增這些 TAILSPINTOYS 網域中的系統管理員 account 拒絕權限，您可以瀏覽以系統管理員負責 TAILSPINTOYS 網域中，它會顯示為 TAILSPINTOYS\Administrator。 如果您輸入**系統管理員**中的這些使用者權限設定群組原則物件編輯器] 中，您將會限制 GPO 所套用的每一部電腦上本機系統管理員 account 之前所述。  

10. 設定使用者權限以避免本機存取成員伺服器，並透過遠端桌面服務工作站，方法如下：  

    1.  按兩下**透過遠端桌面服務拒絕登入**，然後選取**定義這些原則設定**。  

    2.  按一下**新增使用者或群組**，輸入本機的使用者名稱，然後按**[確定]**。 此使用者名稱將會**系統管理員**時已安裝 Windows,，預設值。  

        ![安全本機系統管理員帳號，並群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)  

    3.  按一下**[確定]**。  

        > [!IMPORTANT]  
        > 當您新增這些設定的系統管理員帳號時，您可以指定是否您所設定的本機或核對系統管理員的帳號的標籤。 例如，若要新增這些 TAILSPINTOYS 網域中的系統管理員 account 拒絕權限，您可以瀏覽以系統管理員負責 TAILSPINTOYS 網域中，它會顯示為 TAILSPINTOYS\Administrator。 如果您輸入**系統管理員**中的這些使用者權限設定群組原則物件編輯器] 中，您將會限制 GPO 所套用的每一部電腦上本機系統管理員 account 之前所述。  

11. 結束**群組原則編輯器] 管理**，按一下 [**檔案**，並按**結束**。  

12. 在**群組原則管理**，將 GPO 連結到工作站 Ou 與成員伺服器，方法如下：  

    1.  瀏覽至<Forest>\Domains\\<Domain> (其中<Forest>是樹系的名稱及<Domain>是您想要設定群組原則設定的網域名稱)。  

    2.  以滑鼠右鍵按一下組織單位，將會套用至 GPO，然後按一下**的現有 GPO 連結**。  

        ![安全本機系統管理員帳號，並群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)  

    3.  選取 [建立 GPO 並按一下**[確定]**。  

        ![安全本機系統管理員帳號，並群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)  

    4.  建立包含工作站所有其他 Ou 的連結。  

    5.  建立所有其他 Ou 包含成員伺服器的連結。  

#### <a name="verification-steps"></a>步驟驗證  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>請檢查 「 Deny 從網路存取此電腦] GPO 設定  

從任何成員伺服器或 GPO 變更 （例如捷徑伺服器） 不會受到影響的工作站，嘗試透過受 GPO 變更網路存取成員伺服器或工作站。 要檢查 GPO 設定，請嘗試將系統磁碟機對應使用**網路使用**命令。  

1.  登入本機任何成員伺服器或 GPO 變更不會受到影響的工作站。  

2.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

3.  在**搜尋**方塊中，輸入**命令提示字元**，以滑鼠右鍵按一下**命令提示字元**，，然後按一下**以系統管理員身分執行**打開提升權限的命令提示字元。  

4.  核准提高權限提示，請按一下**[是]**。  

    ![安全本機系統管理員帳號，並群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)  

5.  在**命令提示字元**視窗中，輸入**網路使用 \\\<Server Name>\c$ /user:<Server Name>\Administrator**，其中<Server Name>是您嘗試在網路上存取的工作站成員伺服器的名稱。  

    > [!NOTE]  
    > 必須是相同的系統您嘗試在網路上存取本機系統管理員認證。  

6.  下圖顯示應該會出現錯誤訊息。  

    ![安全本機系統管理員帳號，並群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認 [拒絕登入分批為 「 GPO 設定  
從任何成員伺服器或受到 GPO 變更工作站，登入本機。  

###### <a name="create-a-batch-file"></a>建立批次檔案  

1.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

2.  在**搜尋**方塊中，輸入**「 記事本 」**，並按**記事本**。  

3.  在**[記事本]**，輸入**dir c:**。  

4.  按一下**檔案**，按一下 [**儲存為**。  

5.  在**檔案名稱**方塊中，輸入** <Filename>.bat** (其中<Filename>是新的 「 批次檔案的名稱)。  

###### <a name="schedule-a-task"></a>排程工作  

1.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

2.  在**搜尋**方塊中輸入工作排程器，然後按**工作排程器**。  

    > [!NOTE]  
    > 在電腦上執行 Windows 8 的**搜尋**方塊中，輸入**排程工作**，並按**排程工作**。  

3.  按一下**動作**，按一下 [**建立工作**。  

4.  在**建立工作**對話方塊中，輸入** <Task Name> ** (其中<Task Name>是新工作的名稱)。  

5.  按一下**動作**索引標籤，然後按**新增]**。  

6.  在**動作**欄位中，按一下 [**開始程式]**。  

7.  在**程式日指令碼**欄位中，按一下 [**瀏覽]**，找出並選取 [建立在 「 批次檔案**建立批次檔案**區段，然後按一下**開放**。  

8.  按一下**[確定]**。  

9. 按一下**一般**索引標籤。  

10. 在**安全性選項**欄位中，按**變更使用者或群組**。  

11. 輸入本機系統管理員的名稱，請按一下**檢查名稱]**，按一下 [ **[確定]**。  

12. 選取 [**是否使用者登入或不執行**和**不要儲存密碼**。 任務將只可以存取本機電腦資源。  

13. 按一下**[確定]**。  

14. 應該會出現一個對話方塊，要求帳號認證執行的工作。  

15. 輸入認證之後, 請按**[確定]**。  

16. 應該會出現一個對話方塊類似下列。  

    ![安全本機系統管理員帳號，並群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)  

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認 [拒絕登入即服務 」 GPO 設定  

1.  從任何成員伺服器或受到 GPO 變更工作站，登入本機。  

2.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

3.  在**搜尋**方塊中，輸入**服務**，並按**服務**。  

4.  找出並按兩下 [**列印多工緩衝處理器**。  

5.  按一下**登入**索引標籤。  

6.  在**以登入**欄位中，按一下 [**此 account**。  

7.  按一下**瀏覽**，輸入本機系統管理員，按一下 [**檢查名稱**，並按一下 [ **[確定]**。  

8.  在**密碼**和**確認密碼**欄位，輸入所選取的密碼，然後按一下 [ **[確定]**。  

9. 按一下**[確定]**三次。  

10. 以滑鼠右鍵按一下**列印多工緩衝處理器**，按一下 [**重新開機**。  

11. 服務會重新開始時，應該會顯示對話方塊中，如下所示。  

    ![安全本機系統管理員帳號，並群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)  

###### <a name="revert-changes-to-the-printer-spooler-service"></a>還原已變更的印表機多工緩衝處理器服務  

1.  從任何成員伺服器或受到 GPO 變更工作站，登入本機。  

2.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

3.  在**搜尋**方塊中，輸入**服務**，並按**服務**。  

4.  找出並按兩下 [**列印多工緩衝處理器**。  

5.  按一下**登入**索引標籤。  

6.  在**身分登入**： 欄位中，選取**本機 Systemaccount**，並按**[確定]**。  

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>確認 [拒絕登入遠端桌面服務透過 「 GPO 設定  

1.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

2.  在**搜尋**方塊中，輸入**遠端桌面連接**，並按**遠端桌面連接**。  

3.  在**電腦**欄位中，輸入您想要連接，然後按一下電腦名稱**連接**。 （您也可以輸入 IP 位址，而不是電腦名稱）。  

4.  出現提示時，提供的認證系統的當地的**系統管理員**account。  

5.  應該會出現一個對話方塊類似下列。  

    ![安全本機系統管理員帳號，並群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)  

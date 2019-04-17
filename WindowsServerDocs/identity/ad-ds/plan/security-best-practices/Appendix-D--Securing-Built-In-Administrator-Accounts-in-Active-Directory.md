---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: "附錄 D-保護建系統管理員帳號 Active Directory 中"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2878e661e1bb93fcdc3161c46b73d8e4baec76d2
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/13/2018
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>在 Active Directory 中附錄 d 保護建系統管理員帳號

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>在 Active Directory 中附錄 d 保護建系統管理員帳號  
在 Active Directory 中每個網域中建立管理員建立網域中的一部分。 這個 account 預設成員網域系統管理員 」 及系統管理員網域中的群組，而且如果網域是森林根網域，account 是也的企業系統管理員群組成員。

應只適用於初始建置活動，而且可能損壞修復案例保留網域管理員使用。 若要確保管理員，可以用於影響替換，可以使用任何其他帳號，您不應該變更管理員森林中的任何網域中的預設成員資格。 您應該安全森林中的每個網域中的系統管理員帳號下, 一節中所述而逐步指示，請依照下列所述。 

> [!NOTE]
> 本指南使用建議停用 account。 此白皮書樹系修復為已移除預設管理員使用。 是，這是讓登入，而不需要全球 Catalog 伺服器只 account。


#### <a name="controls-for-built-in-administrator-accounts"></a>控制帳號建系統管理員  
適用於建您森林中的每個網域中，您應該進行下列設定：  

-   讓**機密帳號，無法委派**上 account 旗標。  

-   讓**智慧卡，才互動式登入**上 account 旗標。  

-   設定限制加入網域的系統管理員使用 Gpo:  

    -   在您建立和連結工作站和成員伺服器 Ou 每個網域中的一或多個 Gpo，將每個網域的管理員新增至下列使用者權限在**電腦設定 \ 原則 \windows 安全性設定本機 Settings\User 權限指派**:  

        -   拒絕從網路存取此電腦  

        -   拒絕以分批登入  

        -   拒絕登入即服務  

        -   透過遠端桌面服務拒絕登入  

> [!NOTE]  
> 當您新增此設定帳號時，您必須指定您要設定本機系統管理員帳號或網域系統管理員帳號。 例如，將這些 NWTRADERS 網域中的系統管理員 account 拒絕權限，您必須輸入 account NWTRADERS\Administrator 或瀏覽至管理員 NWTRADERS 網域。 如果您在這些使用者權限設定群組原則物件編輯器中輸入 「 系統管理員 」，您將會限制本機管理員 GPO 所套用的每一部電腦上。
>   
> 我們建議以相同的方式為網域型系統管理員帳號限制本機系統管理員帳號工作站成員伺服器上。 因此，您應該通常加入每個網域森林中的系統管理員負責和本機電腦的系統管理員負責這些使用者權限設定。 下圖顯示設定封鎖本機系統管理員帳號，並網域的管理員，執行下列帳號的應該不需要登入的這些使用者權限的範例。  


![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   設定限制的系統管理員帳號網域控制站 Gpo  
    -   森林中的每個網域中預設的網域控制站 GPO 或原則連結到網域控制站組織單位應修改將每個網域的管理員新增至下列使用者權限在**電腦設定 \ 原則 \windows 安全性設定本機 Settings\User 權限指派**:   
        -   拒絕從網路存取此電腦  

        -   拒絕以分批登入  

        -   拒絕登入即服務  

        -   透過遠端桌面服務拒絕登入  

> [!NOTE]  
> 這些設定將可確保您的網域的建不能用來連接網域控制站，雖然帳號，如果功能，可以登入本機網域控制站。 應該僅支援並損壞修復案例中使用此帳號，因為它被預期實體存取至少網域控制站將或從遠端存取網域控制站的權限的其他帳號，可以使用。  

-   設定的系統管理員帳號稽核  

    當您有保護每個網域中的系統管理員帳號，並將它關閉時，您應該設定稽核監視 account 變更。 如果尚未 account、 重設密碼，或任何其他修改過去，應該會收到通知小組負責管理 Active directory，除了事件回應團隊，在組織中的使用者。  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>逐步指示保護建系統管理員帳號 Active Directory 中  

1.  在**伺服器管理員**，按一下 [**工具**，並按一下 [ **Active Directory 使用者和電腦**。  

2.  若要防止利用其他系統上使用 account 的認證委派的攻擊，執行下列步驟：  

    1.  以滑鼠右鍵按一下**系統管理員**帳號，按**屬性**。  

    2.  按一下**Account**索引標籤。  

    3.  在**帳號選項**，請選取**機密帳號，無法委派**標示 （如同指示），下列螢幕擷取畫面，並按**[確定]**。  

        ![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  若要讓**智慧卡，才互動式登入**旗標帳號，請執行下列步驟：  

    1.  以滑鼠右鍵按一下**系統管理員**帳號，並選取 [**屬性**。  

    2.  按一下**Account**索引標籤。  

    3.  在**Account**選項，選取**智慧卡，才互動式登入**標示 （如同指示），下列螢幕擷取畫面，並按**[確定]**。  

        ![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>設定限制的系統管理員帳號網域層級 Gpo  

> [!WARNING]  
> 永遠不會應該將此 GPO 連結網域層級，因為這會使建無法使用，甚至在損壞復原案例中。  

1.  在**伺服器管理員**，按一下 [**工具**，並按**群組原則管理**。  

2.  在主控台中，展開<Forest>\Domains\\<Domain>，然後**群組原則物件**(其中<Forest>樹系的名稱和<Domain>是您想要用來建立群組原則的網域名稱)。  

3.  在主機上按一下滑鼠右鍵**群組原則物件**，按一下 [**新增]**。  

    ![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  在**新的 GPO**對話方塊中，輸入<GPO Name>，按一下**[確定]** (其中<GPO Name>是此 GPO 的名稱) （如同指示），下列螢幕擷取畫面。  

    ![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  在詳細資料窗格中，以滑鼠右鍵按一下<GPO Name>，並按一下 [**編輯**。  

6.  瀏覽至**電腦設定 \ 原則 \windows 安全性設定本機原則**，按一下 [**權限指派使用者]**。  

    ![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  設定使用者權限以避免管理員成員伺服器和工作站網路存取，方法如下：  

    1.  按兩下**拒絕從網路存取這台電腦**，然後選取**定義這些原則設定**。  

    2.  按一下**[新增使用者或群組**，按一下 [**瀏覽]**。  

    3.  輸入**系統管理員**，按一下 [**檢查名稱]**，並按一下 [ **[確定]**。 請確認 account 會顯示在<DomainName>\Username 格式 （如同指示），下列螢幕擷取畫面。  

        ![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  按一下**[確定]**，以及**[確定]**一次。  

8.  設定使用者權限以避免管理員分批身分登入，方法如下：  

    1.  按兩下**拒絕以分批登入**，然後選取**定義這些原則設定**。  

    2.  按一下**[新增使用者或群組**，按一下 [**瀏覽]**。  

    3.  輸入**系統管理員**，按一下 [**檢查名稱]**，並按一下 [ **[確定]**。 請確認 account 會顯示在<DomainName>\Username 格式 （如同指示），下列螢幕擷取畫面。  

        ![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  按一下**[確定]**，以及**[確定]**一次。  

9. 設定使用者防止管理員執行以下動作來登入以服務的權限：  

    1.  按兩下**以服務拒絕登入**，然後選取**定義這些原則設定**。  

    2.  按一下**[新增使用者或群組**，按一下 [**瀏覽]**。  

    3.  輸入**系統管理員**，按一下 [**檢查名稱]**，並按一下 [ **[確定]**。 請確認 account 會顯示在<DomainName>\Username 格式 （如同指示），下列螢幕擷取畫面。  

        ![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  按一下**[確定]**，以及**[確定]**一次。  

10. 設定使用者權限以避免 BA account 存取成員伺服器，並透過遠端桌面服務工作站，方法如下：  

    1.  按兩下**透過遠端桌面服務拒絕登入**，然後選取**定義這些原則設定**。  

    2.  按一下**[新增使用者或群組**，按一下 [**瀏覽]**。  

    3.  輸入**系統管理員**，按一下 [**檢查名稱]**，並按一下 [ **[確定]**。 請確認 account 會顯示在<DomainName>\Username 格式 （如同指示），下列螢幕擷取畫面。  

        ![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  按一下**[確定]**，以及**[確定]**一次。  

11. 結束**群組原則編輯器] 管理**，按一下 [**檔案**，並按**結束**。  

12. 在**群組原則管理**，將 GPO 連結到工作站 Ou 與成員伺服器，方法如下：  

    1.  瀏覽至<Forest>\Domains\\<Domain> (其中<Forest>是樹系的名稱及<Domain>是您想要設定群組原則設定的網域名稱)。  

    2.  以滑鼠右鍵按一下組織單位，將會套用至 GPO，然後按一下**的現有 GPO 連結**。  

        ![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  選取 [建立 GPO 並按一下**[確定]**。  

        ![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  建立包含工作站所有其他 Ou 的連結。  

    5.  建立所有其他 Ou 包含成員伺服器的連結。  

> [!IMPORTANT]  
> 當您新增這些設定的系統管理員帳號時，您可以指定是否您所設定的本機或核對系統管理員的帳號的標籤。 例如，若要新增這些 TAILSPINTOYS 網域中的系統管理員 account 拒絕權限，您可以瀏覽以系統管理員負責 TAILSPINTOYS 網域中，它會顯示為 TAILSPINTOYS\Administrator。 如果您在這些使用者權限設定群組原則物件編輯器中輸入 「 系統管理員 」，您將會限制 GPO 所套用的每一部電腦上本機系統管理員 account 之前所述。  

#### <a name="verification-steps"></a>步驟驗證  
以下簡述的驗證步驟只適用於 Windows 8 和 Windows Server 2012。  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>確認 「 智慧卡需互動式登入的 「 Account 選項  

1.  從任何成員伺服器或工作站受到 GPO 變更，嘗試登入互動方式網域使用的網域建。 在嘗試登入後, 對話方塊類似下列應該會出現。  

![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>確認 [Account 已停用 「 Account 選項  

1.  從任何成員伺服器或工作站受到 GPO 變更，嘗試登入互動方式網域使用的網域建。 在嘗試登入後, 對話方塊類似下列應該會出現。  

![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>請檢查 「 Deny 從網路存取此電腦] GPO 設定  
從任何成員伺服器或 GPO 變更 （例如捷徑伺服器） 不會受到影響的工作站，嘗試透過受 GPO 變更網路存取成員伺服器或工作站。 要檢查 GPO 設定，請嘗試將系統磁碟機對應使用**網路使用**命令執行下列步驟：  

1.  登入的網域建網域。  

2.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

3.  在**搜尋**方塊中，輸入**命令提示字元**，以滑鼠右鍵按一下**命令提示字元**，，然後按一下**以系統管理員身分執行**打開提升權限的命令提示字元。  

4.  核准提高權限提示，請按一下**[是]**。  

    ![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  在**命令提示字元**視窗中，輸入**網路使用 \\\<Server Name>\c$**，其中<Server Name>是您正嘗試在網路上存取的工作站成員伺服器的名稱。  

6.  下圖顯示應該會出現錯誤訊息。  

    ![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認 [拒絕登入分批為 「 GPO 設定  

從任何成員伺服器或受到 GPO 變更工作站，登入本機。  

###### <a name="create-a-batch-file"></a>建立批次檔案  

1.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

2.  在**搜尋**方塊中，輸入**「 記事本 」**，並按**記事本**。  

3.  在**[記事本]**，輸入**dir c:**。  

4.  按一下**檔案**，按一下 [**儲存為**。  

5.  在**檔名**欄位中，輸入** <Filename>.bat** (其中<Filename>是新的 「 批次檔案的名稱)。  

###### <a name="schedule-a-task"></a>排程工作  

1.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

2.  在**搜尋**方塊中，輸入**工作排程器**，並按**工作排程器**。  

    > [!NOTE]  
    > 在 [電腦是執行 Windows 8，在搜尋方塊中，輸入**排程工作**，按一下 [**排程工作**。  

3.  在**工作排程器**，按一下 [**動作**，並按一下 [**建立工作**。  

4.  在**建立工作**對話方塊中，輸入** <Task Name> ** (其中** <Task Name> **新工作的名稱)。  

5.  按一下**動作**索引標籤，然後按**新增]**。  

6.  在**動作：**，請選取**開始程式]**。  

7.  在**程式日指令碼：**，按一下 [**瀏覽]**，找出並選取 [建立批次檔案 」 一節中所建立的批次檔案，按一下 [**開放**。  

8.  按一下**[確定]**。  

9. 按一下**一般**索引標籤。  

10. 在**安全性**按一下 [選項]**變更使用者或群組**。  

11. 輸入 BA account 的名稱，在網域層級，請按**檢查名稱**，按一下 [ **[確定]**。  

12. 選取 [**是否使用者登入或不執行**和**不要儲存密碼**。 任務將只可以存取本機電腦資源。  

13. 按一下**[確定]**。  

14. 應該會出現一個對話方塊，要求帳號認證執行的工作。  

15. 輸入認證之後, 請按**[確定]**。  

16. 應該會出現一個對話方塊類似下列。  

    ![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認 [拒絕登入即服務 」 GPO 設定  

1.  從任何成員伺服器或受到 GPO 變更工作站，登入本機。  

2.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

3.  在**搜尋**方塊中，輸入**服務**，並按**服務**。  

4.  找出並按兩下 [**列印多工緩衝處理器**。  

5.  按一下**登入**索引標籤。  

6.  在**以登入：**，請選取**此 account**。  

7.  按一下**瀏覽**，輸入名稱 BA account 網域層級，按一下 [**檢查名稱**，並按一下 [ **[確定]**。  

8.  在**的密碼：**和**確認密碼：**、 輸入管理員密碼，然後按一下 [ **[確定]**。  

9. 按一下**[確定]**三次。  

10. 以滑鼠右鍵按一下**列印多工緩衝處理器服務**，然後選取**重新開機**。  

11. 服務會重新開始時，應該會顯示對話方塊中，如下所示。  

    ![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>還原已變更的印表機多工緩衝處理器服務  

1.  從任何成員伺服器或受到 GPO 變更工作站，登入本機。  

2.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

3.  在**搜尋**方塊中，輸入**服務**，並按**服務**。  

4.  找出並按兩下 [**列印多工緩衝處理器**。  

5.  按一下**登入**索引標籤。  

6.  在**以登入：**，請選取**本機系統**帳號，並按**[確定]**。  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>確認 [拒絕登入遠端桌面服務透過 「 GPO 設定

1.  使用滑鼠，將滑鼠指標移動到畫面的右上角或右下角。 當**常用**列出現時，按**搜尋**。  

2.  在**搜尋**方塊中，輸入**遠端桌面連接**，並按**遠端桌面連接**。  

3.  在**電腦**欄位中，輸入您想要連接，然後按一下電腦名稱**連接**。 （您也可以輸入 IP 位址，而不是電腦名稱）。  

4.  出現提示時，提供的認證 BA account 網域層級的名稱。  

5.  應該會出現一個對話方塊類似下列。  

    ![保護建管理員帳號](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  

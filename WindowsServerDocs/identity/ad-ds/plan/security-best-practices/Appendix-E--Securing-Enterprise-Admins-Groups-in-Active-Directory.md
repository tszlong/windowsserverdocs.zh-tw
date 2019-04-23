---
ms.assetid: f643099e-f9c6-476f-9378-5a9228c39b33
title: 附錄 E-保護 Active Directory 中的 Enterprise Admins 群組
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 976eb8c7159c8349b72bee05a5248b5cc116d96b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856719"
---
# <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>附錄 E：保護 Active Directory 中的 Enterprise Admins 群組

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012


## <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>附錄 E：保護 Active Directory 中的 Enterprise Admins 群組  
Enterprise Admins (EA) 群組，該表格位於樹系根網域中，應該包含在日常，與可能的例外狀況的根網域的系統管理員帳戶，沒有任何使用者，提供保護中所述[附錄 d:保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

企業系統管理員 」 是，根據預設，每個樹系中網域系統管理員群組的成員。 您不應該移除每個網域中的系統管理員群組從 EA 群組，因為樹系災害復原案例中，萬一 EA 權限可能會需要。 應該保護樹系的 Enterprise Admins 群組中的逐步指示，請依照下列所述。  

樹系中 Enterprise Admins 群組：  

1.  在 Gpo 連結到 Ou 包含成員伺服器和工作站，每個網域中的，Enterprise Admins 群組應該新增到下列中的使用者權限**電腦設定 \ 原則 \windows 設定 \ 安全性設定 \ 本機 Policies\使用者權限指派**:  

    -   拒絕從網路存取這台電腦  

    -   拒絕以批次工作登入  

    -   拒絕以服務方式登入  

    -   拒絕本機登入  

    -   拒絕透過遠端桌面服務登入  

2.  設定稽核，傳送警示，如果對屬性或 Enterprise Admins 群組的成員資格進行任何修改。  

### <a name="step-by-step-instructions-for-removing-all-members-from-the-enterprise-admins-group"></a>從 Enterprise Admins 群組中移除所有成員的逐步指示  

1.  在**伺服器管理員**，按一下**工具**，然後按一下**Active Directory 使用者和電腦**。  

2.  如果您不想要管理根網域樹系中，在主控台樹狀目錄中，以滑鼠右鍵按一下<Domain>，然後按一下**變更網域**(其中<Domain>是您目前正在管理的網域名稱)。  

    ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_43.gif)  

3.  在 [**變更網域**] 對話方塊中，按一下**瀏覽**，選取樹系根網域，然後按一下 **[確定]**。  

    ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_44.gif)  

4.  若要從 EA 群組中移除所有成員：  

    1.  按兩下**Enterprise Admins**群組，然後按一下**成員** 索引標籤。  

        ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_45.gif)  

    2.  選取群組的成員，然後按一下**移除**，按一下**是**，然後按一下**確定**。  

5.  在移除所有群組成員的 EA 之前，請重複步驟 2。  

### <a name="step-by-step-instructions-to-secure-enterprise-admins-in-active-directory"></a>保護 Active Directory 中的 Enterprise Admins 的逐步指示  

1.  在 **伺服器管理員**，按一下**工具**，然後按一下**群組原則管理**。  

2.  在主控台樹狀目錄中，依序展開<Forest>\Domains\\<Domain>，然後**群組原則物件**(其中<Forest>樹系的名稱和<Domain>是您想要的網域名稱群組原則設定）。  

    > [!NOTE]  
    > 在包含多個網域的樹系應該需要 Enterprise Admins 群組來保護每個網域中建立類似的 GPO。  

3.  在主控台樹狀目錄中，以滑鼠右鍵按一下**群組原則物件**，然後按一下**新增**。  

    ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_46.gif)  

4.  在**新的 GPO**  對話方塊中，輸入<GPO Name>，然後按一下**確定** (其中<GPO Name>此 GPO 的名稱)。  

    ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_47.gif)  

5.  在 [詳細資料] 窗格中，以滑鼠右鍵按一下<GPO Name>，然後按一下**編輯**。  

6.  瀏覽至**電腦設定 \ 原則 \windows 設定 \ 本機原則**，然後按一下**使用者權限指派**。  

    ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_48.gif)  

7.  設定使用者權限，以防止 Enterprise Admins 群組的成員透過網路存取成員伺服器和工作站，執行下列動作：  

    1.  按兩下**拒絕從網路存取這台電腦**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

    3.  型別**Enterprise Admins**，按一下**檢查名稱**，然後按一下**確定**。  

        ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_49.gif)  

    4.  按一下  **確定**，並**確定**一次。  

8.  設定使用者權限，以防止 Enterprise Admins 群組的成員無法以批次工作登入執行下列動作：  

    1.  按兩下**拒絕以批次工作登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

        > [!NOTE]  
        > 在包含多個網域樹系中，按一下**位置**和選取的樹系根網域。  

    3.  型別**Enterprise Admins**，按一下**檢查名稱**，然後按一下**確定**。  

        ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_50.gif)  

    4.  按一下  **確定**，並**確定**一次。  

9. 設定使用者權限，以防止 EA 群組的成員登入為服務執行下列動作：  

    1.  按兩下**拒絕以服務登**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**，然後按一下**瀏覽**。  

        > [!NOTE]  
        > 在包含多個網域樹系中，按一下**位置**和選取的樹系根網域。  

    3.  型別**Enterprise Admins**，按一下**檢查名稱**，然後按一下**確定**。  

        ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_51.gif)  

    4.  按一下  **確定**，並**確定**一次。  

10. 設定使用者權限，以防止 Enterprise Admins 群組的成員登入本機成員伺服器和工作站執行下列動作：  

    1.  按兩下**拒絕本機登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**，然後按一下**瀏覽**。  

        > [!NOTE]  
        > 在包含多個網域樹系中，按一下**位置**和選取的樹系根網域。  

    3.  型別**Enterprise Admins**，按一下**檢查名稱**，然後按一下**確定**。  

        ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_52.gif)  

    4.  按一下  **確定**，並**確定**一次。  

11. 設定使用者權限，以防止 Enterprise Admins 群組的成員存取成員伺服器和工作站，透過遠端桌面服務，執行下列動作：  

    1.  按兩下**拒絕透過遠端桌面服務登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**，然後按一下**瀏覽**。  

        > [!NOTE]  
        > 在包含多個網域樹系中，按一下**位置**和選取的樹系根網域。  

    3.  型別**Enterprise Admins**，按一下**檢查名稱**，然後按一下**確定**。  

        ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_53.gif)  

    4.  按一下  **確定**，並**確定**一次。  

12. 若要結束**群組原則管理編輯器**，按一下**檔案**，然後按一下**結束**。  

13. 在 **群組原則管理**，將 GPO 連結到成員伺服器和工作站的 Ou，執行下列動作：  

    1.  瀏覽至<Forest>\Domains\\ <Domain> (其中<Forest>樹系的名稱和<Domain>是您要將群組原則設定的網域名稱)。  

    2.  以滑鼠右鍵按一下 OU GPO 會套用至，然後按一下**連結到現有的 GPO**。  

        ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_54.gif)  

    3.  選取您剛才建立的 GPO，然後按一下**確定**。  

        ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_55.gif)  

    4.  建立連結至包含工作站的所有其他 Ou。  

    5.  建立連結至包含成員伺服器的所有其他 Ou。  

    6.  在包含多個網域的樹系應該需要 Enterprise Admins 群組來保護每個網域中建立類似的 GPO。  

> [!IMPORTANT]  
> 如果跳躍伺服器用來管理網域控制站和 Active Directory，請確定跳躍伺服器都位於此未連結 Gpo OU 中。  

### <a name="verification-steps"></a>驗證步驟  

#### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>確認 「 拒絕從網路存取這台電腦 」 GPO 設定  
從任何成員伺服器或工作站 GPO 變更 （例如 「 跳躍伺服器 」） 不會受到影響，嘗試存取的成員伺服器或工作站 GPO 變更的影響網路上。 若要確認 GPO 設定，請嘗試使用對應的系統磁碟機**NET USE**命令藉由執行下列步驟：  

1.  在本機使用 EA 群組的成員帳戶登入。  

2.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

3.  在**搜尋**方塊中，輸入**命令提示字元**，以滑鼠右鍵按一下**命令提示字元**，然後按一下**系統管理員身分執行**開啟已提升權限命令提示字元。  

4.  當系統提示您核准提升權限，按一下**是**。  

    ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_56.gif)  

5.  在 **命令提示字元**視窗中，輸入**net 使用\\ \\\<伺服器名稱\>\c$**，其中\<伺服器名稱\>是成員伺服器或您嘗試透過網路存取的工作站的名稱。  

6.  下列螢幕擷取畫面會顯示應該會出現錯誤訊息。  

    ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_57.gif)  

#### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認 「 拒絕登入以批次工作 」 GPO 設定  

從任何成員伺服器或工作站 GPO 變更的影響，在本機登入。  

##### <a name="create-a-batch-file"></a>建立批次檔  

1.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

2.  中**搜尋**方塊中，輸入**記事本**，然後按一下**記事本**。  

3.  在 **記事本**，型別**dir c:**。  

4.  按一下 **檔案**，然後按一下**另存新檔**。  

5.  在 **檔案**名稱方塊中，輸入 **<Filename>.bat** (其中<Filename>是新的批次檔的名稱)。  

##### <a name="schedule-a-task"></a>排程的工作  

1.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

2.  在 **搜尋**方塊中，輸入**工作排程器**，然後按一下**工作排程器**。  

    > [!NOTE]  
    > 在電腦上執行 Windows 8 中，在**搜尋**方塊中，輸入**排程工作**，然後按一下**排程工作**。  

3.  按一下 **動作**，然後按一下**建立工作**。  

4.  在 [**建立工作**] 對話方塊中，輸入**<Task Name>** (其中<Task Name>是新工作的名稱)。  

5.  按一下 **動作**索引標籤，然後按一下**新增**。  

6.  在 **動作**欄位中，選取**啟動程式**。  

7.  底下**程式或指令碼**，按一下**瀏覽**，找出並選取 批次檔中建立**建立批次檔**區段，然後按一下 **開啟**.  

8.  按一下 [確定] 。  

9. 按一下 [一般] 索引標籤。  

10. 在 **安全性選項**欄位中，按一下**變更使用者或群組**。  

11. 輸入 EAs 群組的成員帳戶的名稱，再按一下**檢查名稱**，然後按一下**確定**。  

12. 選取 **不論使用者登入與否均執行**，然後選取**不要儲存密碼**。 工作將只包含本機電腦資源的存取權。  

13. 按一下 [確定] 。  

14. 應該會出現一個對話方塊，要求的使用者帳戶的認證來執行工作。  

15. 輸入認證之後，按一下**確定**。  

16. 應該會出現如下所示的對話方塊。  

    ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_58.gif)  

#### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認 「 拒絕登入為服務 」 GPO 設定  

1.  從任何成員伺服器或工作站 GPO 變更的影響，在本機登入。  

2.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

3.  在 **搜尋**方塊中，輸入**服務**，然後按一下**服務**。  

4.  找出並按兩下**列印多工緩衝處理器**。  

5.  单击 **“登录”** 选项卡。  

6.  底下**身分登入**，選取**此帳戶**。  

7.  按一下 **瀏覽**，輸入 EAs 群組的成員帳戶的名稱，按一下**檢查名稱**，然後按一下**確定**。  

8.  底下**密碼：** 並**確認密碼**，輸入所選的帳戶的密碼，然後按一下**確定**。  

9. 按一下 **確定**三次。  

10. 以滑鼠右鍵按一下**列印多工緩衝處理器**服務，然後選取**重新啟動**。  

11. 重新啟動服務時，應該會出現如下所示的對話方塊。  

    ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_59.gif)  

#### <a name="revert-changes-to-the-printer-spooler-service"></a>還原的列印多工緩衝處理器服務的變更  

1.  從任何成員伺服器或工作站 GPO 變更的影響，在本機登入。  

2.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

3.  在 **搜尋**方塊中，輸入**服務**，然後按一下**服務**。  

4.  找出並按兩下**列印多工緩衝處理器**。  

5.  单击 **“登录”** 选项卡。  

6.  底下**身分登入**，選取**本機系統**帳戶，再按一下**確定**。  

#### <a name="verify-deny-log-on-locally-gpo-settings"></a>確認 「 拒絕本機登入 」 GPO 設定  

1.  從任何成員伺服器或工作站 GPO 變更的影響，嘗試在本機使用 EA 群組的成員帳戶登入。 應該會出現如下所示的對話方塊。  

    ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_60.gif)  

#### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>確認 「 拒絕透過登入遠端桌面服務 」 GPO 設定  

1.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

2.  在 **搜尋**方塊中，輸入**遠端桌面連線**，然後按一下**遠端桌面連線**。  

3.  在 **電腦**欄位中，輸入您想要連接，然後按一下 電腦名稱**Connect**。 （您也可以輸入的 IP 位址，而非電腦名稱）。  

4.  出現提示時，提供 EA 群組成員的帳戶認證。  

5.  應該會出現如下所示的對話方塊。  

    ![保護企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_61.gif)  

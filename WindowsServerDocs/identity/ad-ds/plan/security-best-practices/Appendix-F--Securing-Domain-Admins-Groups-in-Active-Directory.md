---
ms.assetid: 017b88a6-f29b-4787-99b6-b5c8eaf8c3df
title: 附錄 F-保護 Active Directory 中的 Domain Admins 群組
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1f35503d1f02d616255c067fbc1750a0cab974cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880139"
---
# <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>附錄 F：保護 Active Directory 中的 Domain Admins 群組

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012


## <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>附錄 F：保護 Active Directory 中的 Domain Admins 群組  
在此情況下，與 Enterprise Admins (EA) 群組，應該只在建置或災害復原案例中需要網域系統管理員 (DA) 群組的成員資格。 中應該會有任何日常規律的使用者帳戶資料群組，除了內建的 Administrator 帳戶的網域中所述保護[附錄 d:保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

網域系統管理員 」 是，根據預設，所有的成員伺服器和其個別網域中的工作站上本機 Administrators 群組的成員。 這個預設巢狀結構不應該修改為可支援性和災害復原之用。 如果已從本機 Administrators 群組的成員伺服器上移除 Domain Admins，群組應該將每個成員伺服器和網域中的工作站上的 Administrators 群組。 接下來的逐步指示中所述，每個網域的 Domain Admins 群組應該受到保護。  

對於每個樹系中的網域中 Domain Admins 群組：  

1.  移除所有成員的群組，可能的例外狀況的內建的 Administrator 帳戶的網域，提供如中所述保護[附錄 d:保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

2.  在連結到包含成員伺服器和工作站，每個網域中的 Ou 的 Gpo，DA 群組應該新增到下列的使用者權限，在**電腦設定 \ 原則 \windows 設定 \ 原則 \ 使用者權限指派**:  

    -   拒絕從網路存取這台電腦  

    -   拒絕以批次工作登入  

    -   拒絕以服務方式登入  

    -   拒絕本機登入  

    -   拒絕透過遠端桌面服務的使用者權限登入  

3.  稽核應該設定為傳送警示，如果對屬性或 Domain Admins 群組的成員資格進行任何修改。  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-domain-admins-group"></a>從 Domain Admins 群組中移除所有成員的逐步指示  

1.  在**伺服器管理員**，按一下**工具**，然後按一下**Active Directory 使用者和電腦**。  

2.  若要移除的資料群組中的所有成員，執行下列步驟：  

    1.  按兩下**Domain Admins**群組，然後按一下**成員** 索引標籤。  

        ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_62.gif)  

    2.  選取群組的成員，然後按一下**移除**，按一下**是**，然後按一下**確定**。  

3.  在移除所有群組成員的資料之前，請重複步驟 2。  

#### <a name="step-by-step-instructions-to-secure-domain-admins-in-active-directory"></a>保護 Active Directory 中的 Domain Admins 的逐步指示  

1.  在 **伺服器管理員**，按一下**工具**，然後按一下**群組原則管理**。  

2.  在主控台樹狀目錄中，依序展開\<樹系\>\\網域\\\<網域\>，然後**群組原則物件**(其中\<樹系\>樹系名稱並\<網域\>是您要將群組原則設定的網域名稱)。  

3.  在主控台樹狀目錄中，以滑鼠右鍵按一下**群組原則物件**，然後按一下**新增**。  

    ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_63.gif)  

4.  在**新的 GPO**  對話方塊中，輸入\<GPO 名稱\>，然後按一下**確定** (其中\<GPO 名稱\>此 GPO 的名稱)。  

    ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_64.gif)  

5.  在 [詳細資料] 窗格中，以滑鼠右鍵按一下\<GPO 名稱\>，然後按一下**編輯**。  

6.  瀏覽至**電腦設定 \ 原則 \windows 設定 \ 本機原則**，然後按一下**使用者權限指派**。  

    ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_65.gif)  

7.  設定使用者權限，以防止 Domain Admins 群組的成員透過網路存取成員伺服器和工作站，執行下列動作：  

    1.  按兩下**拒絕從網路存取這台電腦**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

    3.  型別**Domain Admins**，按一下**檢查名稱**，然後按一下**確定**。  

        ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_66.gif)  

    4.  按一下  **確定**，並**確定**一次。  

8.  設定使用者權限，以避免資料群組的成員無法以批次工作登入執行下列動作：  

    1.  按兩下**拒絕以批次工作登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

    3.  型別**Domain Admins**，按一下**檢查名稱**，然後按一下**確定**。  

        ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_67.gif)  

    4.  按一下  **確定**，並**確定**一次。  

9. 設定使用者權限，以防止資料群組的成員登入為服務執行下列動作：  

    1.  按兩下**拒絕以服務登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

    3.  型別**Domain Admins**，按一下**檢查名稱**，然後按一下**確定**。  

        ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_68.gif)  

    4.  按一下  **確定**，並**確定**一次。  

10. 設定使用者權限，以防止 Domain Admins 群組的成員登入本機成員伺服器和工作站執行下列動作：  

    1.  按兩下**拒絕本機登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

    3.  型別**Domain Admins**，按一下**檢查名稱**，然後按一下**確定**。  

        ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_69.gif)  

    4.  按一下  **確定**，並**確定**一次。  

11. 設定使用者權限，以防止 Domain Admins 群組的成員存取成員伺服器和工作站，透過遠端桌面服務，執行下列動作：  

    1.  按兩下**拒絕透過遠端桌面服務登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

    3.  型別**Domain Admins**，按一下**檢查名稱**，然後按一下**確定**。  

        ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_70.gif)  

    4.  按一下  **確定**，並**確定**一次。  

12. 若要結束**群組原則管理編輯器**，按一下**檔案**，然後按一下**結束**。  

13. 在群組原則管理，將 GPO 連結到成員伺服器和工作站 Ou 執行下列動作：  

    1.  瀏覽至\<樹系\>\Domains\\\<網域\>(其中\<樹系\>是樹系名稱並\<網域\>的名稱您要將群組原則設定的網域）。  

    2.  以滑鼠右鍵按一下 OU GPO 會套用至，然後按一下**連結到現有的 GPO**。  

        ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_71.gif)  

    3.  選取您剛才建立的 GPO，然後按一下**確定**。  

        ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_72.gif)  

    4.  建立連結至包含工作站的所有其他 Ou。  

    5.  建立連結至包含成員伺服器的所有其他 Ou。  

        > [!IMPORTANT]  
        > 如果跳躍伺服器用來管理網域控制站和 Active Directory，請確定跳躍伺服器都位於此未連結 Gpo OU 中。  

#### <a name="verification-steps"></a>驗證步驟  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>確認 「 拒絕從網路存取這台電腦 」 GPO 設定  
從任何成員伺服器或工作站 GPO 變更 （例如 「 跳躍伺服器 」） 不會受到影響，嘗試存取的成員伺服器或工作站 GPO 變更的影響網路上。 若要確認 GPO 設定，請嘗試使用對應的系統磁碟機**NET USE**命令。  

1.  在本機使用屬於 Domain Admins 群組的成員帳戶登入。  

2.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

3.  在**搜尋**方塊中，輸入**命令提示字元**，以滑鼠右鍵按一下**命令提示字元**，然後按一下**系統管理員身分執行**開啟已提升權限命令提示字元。  

4.  當系統提示您核准提升權限，按一下**是**。  

    ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_73.gif)  

5.  在 **命令提示字元**視窗中，輸入**net 使用\\ \\\<伺服器名稱\>\c$**，其中\<伺服器名稱\>是成員伺服器或您嘗試透過網路存取的工作站的名稱。  

6.  下列螢幕擷取畫面會顯示應該會出現錯誤訊息。  

    ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_74.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認 「 拒絕登入以批次工作 」 GPO 設定  

從任何成員伺服器或工作站 GPO 變更的影響，在本機登入。  

###### <a name="create-a-batch-file"></a>建立批次檔  

1.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

2.  中**搜尋**方塊中，輸入**記事本**，然後按一下**記事本**。  

3.  在 **記事本**，型別**dir c:**。  

4.  按一下 **檔案**，然後按一下**另存新檔**。  

5.  在 [**檔案**名稱] 欄位中，輸入**\<檔名\>.bat** (其中\<檔名\>是新的批次檔的名稱)。  

###### <a name="schedule-a-task"></a>排程的工作  

1.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

2.  在 **搜尋**方塊中，輸入**工作排程器**，然後按一下**工作排程器**。  

    > [!NOTE]  
    > 在電腦上執行 Windows 8 中，在**搜尋**方塊中，輸入**排程工作**，然後按一下**排程工作**。  

3.  中**工作排程器** 功能表列中，按一下**動作**，然後按一下**建立工作**。  

4.  在 [**建立工作**] 對話方塊中，輸入**\<工作名稱\>** (其中\<工作名稱\>是新工作的名稱)。  

5.  按一下 **動作**索引標籤，然後按一下**新增**。  

6.  在 **動作**欄位中，選取**啟動程式**。  

7.  底下**程式或指令碼**，按一下**瀏覽**，找出並選取 批次檔中建立**建立批次檔**區段，然後按一下 **開啟**.  

8.  按一下 [確定] 。  

9. 按一下 [一般] 索引標籤。  

10. 底下**安全性**選項，按一下**變更使用者或群組**。  

11. 輸入 Domain Admins 群組的成員帳戶的名稱，再按一下**檢查名稱**，然後按一下**確定**。  

12. 選取 **不論使用者登入與否均執行**，然後選取**不要儲存密碼**。 工作將只包含本機電腦資源的存取權。  

13. 按一下 [確定] 。  

14. 應該會出現一個對話方塊，要求的使用者帳戶的認證來執行工作。  

15. 輸入認證之後，按一下**確定**。  

16. 應該會出現如下所示的對話方塊。  

    ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_75.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認 「 拒絕登入為服務 」 GPO 設定  

1.  從任何成員伺服器或工作站 GPO 變更的影響，在本機登入。  

2.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

3.  在 **搜尋**方塊中，輸入**服務**，然後按一下**服務**。  

4.  找出並按兩下**列印多工緩衝處理器**。  

5.  单击 **“登录”** 选项卡。  

6.  底下**身分登入**，選取**此帳戶**選項。  

7.  按一下 **瀏覽**，輸入屬於 Domain Admins 群組的成員帳戶的名稱，按一下**檢查名稱**，然後按一下**確定**。  

8.  底下**密碼**並**確認密碼**，輸入所選的帳戶的密碼，然後按一下**確定**。  

9. 按一下 **確定**三次。  

10. 以滑鼠右鍵按一下**列印多工緩衝處理器**然後按一下**重新啟動**。  

11. 重新啟動服務時，應該會出現如下所示的對話方塊。  

    ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_76.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>還原的列印多工緩衝處理器服務的變更  

1.  從任何成員伺服器或工作站 GPO 變更的影響，在本機登入。  

2.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

3.  在 **搜尋**方塊中，輸入**服務**，然後按一下**服務**。  

4.  找出並按兩下**列印多工緩衝處理器**。  

5.  单击 **“登录”** 选项卡。  

6.  底下**身分登入**，選取**本機系統**帳戶，再按一下**確定**。  

##### <a name="verify-deny-log-on-locally-gpo-settings"></a>確認 「 拒絕本機登入 」 GPO 設定  

1.  從任何成員伺服器或工作站 GPO 變更的影響，嘗試在本機使用屬於 Domain Admins 群組的成員帳戶登入。 應該會出現如下所示的對話方塊。  

    ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_77.gif)  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>確認 「 拒絕透過登入遠端桌面服務 」 GPO 設定    
1.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

2.  在 **搜尋**方塊中，輸入**遠端桌面連線**，然後按一下**遠端桌面連線**。  

3.  在 **電腦**欄位中，輸入您想要連接到，然後按一下 電腦名稱**Connect**。 （您也可以輸入的 IP 位址，而非電腦名稱）。  

4.  出現提示時，提供 Domain Admins 群組成員的帳戶認證。  

5.  應該會出現如下所示的對話方塊。  

    ![安全的網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_78.gif)  

---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: 附錄 H-保護本機系統管理員帳戶和群組
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 71eea3f623968172076708dbea34d5bbf4a07684
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858689"
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>附錄 h:保護本機系統管理員帳戶和群組

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>附錄 h:保護本機系統管理員帳戶和群組  
所有版本的 Windows 目前在主流支援，在本機系統管理員帳戶已停用根據預設，可以讓帳戶無法用於傳遞-雜湊和其他認證竊取攻擊。 不過，在環境中包含舊版作業系統，或已在其中啟用本機系統管理員帳戶，這些帳戶可以用於如先前所述散佈成員伺服器和工作站的危害。 接下來的逐步指示中所述，每個本機系統管理員帳戶和群組應該受到保護。  

如需設定內建系統管理員 (BA) 群組的安全性考量的詳細資訊，請參閱[實作最低權限系統管理模型](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)。  

#### <a name="controls-for-local-administrator-accounts"></a>本機系統管理員帳戶的控制項  
本機系統管理員帳戶樹系中每個網域中，您應該設定下列設定：  

-   設定 Gpo 來限制在已加入網域的系統上的網域系統管理員帳戶的使用  
    -   在您建立並連結至工作站和成員伺服器 Ou，每個網域中的一或多個 Gpo，系統管理員帳戶新增到下列中的使用者權限**電腦設定 \ 原則 \windows 設定 \ 安全性設定 \ 本機 Policies\使用者權限指派**:  

        -   拒絕從網路存取這台電腦  

        -   拒絕以批次工作登入  

        -   拒絕以服務方式登入  

        -   拒絕透過遠端桌面服務登入  

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>若要保護的本機 Administrators 群組的逐步指示  

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>設定 Gpo，來限制在已加入網域的系統上的系統管理員帳戶  

1.  在 **伺服器管理員**，按一下**工具**，然後按一下**群組原則管理**。  

2.  在主控台樹狀目錄中，依序展開<Forest>\Domains\\<Domain>，然後**群組原則物件**(其中<Forest>樹系的名稱和<Domain>是您想要的網域名稱群組原則設定）。  

3.  在主控台樹狀目錄中，以滑鼠右鍵按一下**群組原則物件**，然後按一下**新增**。  

    ![安全的本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)  

4.  在**新的 GPO**  對話方塊中，輸入**<GPO Name>**，然後按一下**確定** (其中<GPO Name>此 GPO 的名稱)。  

    ![安全的本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)  

5.  在 [詳細資料] 窗格中，以滑鼠右鍵按一下**<GPO Name>**，然後按一下**編輯**。  

6.  瀏覽至**電腦設定 \ 原則 \windows 設定 \ 本機原則**，然後按一下**使用者權限指派**。  

    ![安全的本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)  

7.  設定使用者權限，以防止本機系統管理員帳戶透過網路存取成員伺服器和工作站，執行下列動作：  

    1.  按兩下**拒絕從網路存取這台電腦**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**，輸入本機系統管理員帳戶的使用者名稱，然後按一下**確定**。 此使用者名稱將會**系統管理員**，安裝 Windows 時，預設值。  

        ![安全的本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)  

    3.  按一下 [確定]。  

        > [!IMPORTANT]  
        > 當您新增的系統管理員帳戶，這些設定時，您可以指定設定的本機系統管理員帳戶或網域系統管理員帳戶由您設定帳戶的標籤。 例如，若要新增這些 TAILSPINTOYS 網域的系統管理員帳戶拒絕權限，您必須瀏覽至網域系統管理員帳戶 TAILSPINTOYS，這會顯示為 TAILSPINTOYS\Administrator。 如果您鍵入**系統管理員**中這些使用者的權限設定群組原則物件編輯器 」 中，您將會限制要套用 GPO，每一部電腦上的本機系統管理員帳戶如先前所述。  

8.  設定使用者權限，以防止本機系統管理員帳戶登入以批次工作執行下列動作：  

    1.  按兩下**拒絕以批次工作登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**，輸入本機系統管理員帳戶的使用者名稱，然後按一下**確定**。 此使用者名稱將會**系統管理員**，安裝 Windows 時，預設值。  

        ![安全的本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)  

    3.  按一下 [確定] 。  

        > [!IMPORTANT]  
        > 當您將系統管理員帳戶新增至這些設定時，指定是否您正在設定本機系統管理員帳戶或網域系統管理員帳戶由您設定帳戶的標籤。 例如，若要新增這些 TAILSPINTOYS 網域的系統管理員帳戶拒絕權限，您必須瀏覽至網域系統管理員帳戶 TAILSPINTOYS，這會顯示為 TAILSPINTOYS\Administrator。 如果您鍵入**系統管理員**中這些使用者的權限設定群組原則物件編輯器 」 中，您將會限制要套用 GPO，每一部電腦上的本機系統管理員帳戶如先前所述。  

9. 設定使用者權限，以防止本機系統管理員帳戶登入為服務執行下列動作：  

    1.  按兩下**拒絕以服務登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**，輸入本機系統管理員帳戶的使用者名稱，然後按一下**確定**。 此使用者名稱將會**系統管理員**，安裝 Windows 時，預設值。  

        ![安全的本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)  

    3.  按一下 [確定] 。  

        > [!IMPORTANT]  
        > 當您將系統管理員帳戶新增至這些設定時，指定是否您正在設定本機系統管理員帳戶或網域系統管理員帳戶由您設定帳戶的標籤。 例如，若要新增這些 TAILSPINTOYS 網域的系統管理員帳戶拒絕權限，您必須瀏覽至網域系統管理員帳戶 TAILSPINTOYS，這會顯示為 TAILSPINTOYS\Administrator。 如果您鍵入**系統管理員**中這些使用者的權限設定群組原則物件編輯器 」 中，您將會限制要套用 GPO，每一部電腦上的本機系統管理員帳戶如先前所述。  

10. 設定使用者權限，以防止本機系統管理員帳戶透過下列方式存取成員伺服器和工作站，透過遠端桌面服務：  

    1.  按兩下**拒絕透過遠端桌面服務登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**，輸入本機系統管理員帳戶的使用者名稱，然後按一下**確定**。 此使用者名稱將會**系統管理員**，安裝 Windows 時，預設值。  

        ![安全的本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)  

    3.  按一下 [確定] 。  

        > [!IMPORTANT]  
        > 當您將系統管理員帳戶新增至這些設定時，指定是否您正在設定本機系統管理員帳戶或網域系統管理員帳戶由您設定帳戶的標籤。 例如，若要新增這些 TAILSPINTOYS 網域的系統管理員帳戶拒絕權限，您必須瀏覽至網域系統管理員帳戶 TAILSPINTOYS，這會顯示為 TAILSPINTOYS\Administrator。 如果您鍵入**系統管理員**中這些使用者的權限設定群組原則物件編輯器 」 中，您將會限制要套用 GPO，每一部電腦上的本機系統管理員帳戶如先前所述。  

11. 若要結束**群組原則管理編輯器**，按一下**檔案**，然後按一下**結束**。  

12. 在 **群組原則管理**，將 GPO 連結到成員伺服器和工作站的 Ou，執行下列動作：  

    1.  瀏覽至<Forest>\Domains\\ <Domain> (其中<Forest>樹系的名稱和<Domain>是您要將群組原則設定的網域名稱)。  

    2.  以滑鼠右鍵按一下 OU GPO 會套用至，然後按一下**連結到現有的 GPO**。  

        ![安全的本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)  

    3.  選取您所建立的 GPO，然後按一下**確定**。  

        ![安全的本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)  

    4.  建立連結至包含工作站的所有其他 Ou。  

    5.  建立連結至包含成員伺服器的所有其他 Ou。  

#### <a name="verification-steps"></a>驗證步驟  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>確認 「 拒絕從網路存取這台電腦 」 GPO 設定  

從任何成員伺服器或工作站 GPO 變更 （例如跳躍伺服器） 不會受到影響，嘗試存取的成員伺服器或工作站 GPO 變更的影響網路上。 若要確認 GPO 設定，請嘗試使用對應的系統磁碟機**NET USE**命令。  

1.  本機登入任何成員伺服器或工作站 GPO 變更不會受到影響。  

2.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

3.  在**搜尋**方塊中，輸入**命令提示字元**，以滑鼠右鍵按一下**命令提示字元**，然後按一下**系統管理員身分執行**開啟已提升權限命令提示字元。  

4.  當系統提示您核准提升權限，按一下**是**。  

    ![安全的本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)  

5.  在 **命令提示字元**視窗中，輸入**net 使用\\ \\ <Server Name>\c$ /user:<Server Name>\Administrator**，其中<Server Name>是成員的名稱伺服器或工作站您嘗試透過網路存取。  

    > [!NOTE]  
    > 本機系統管理員認證必須是從相同的系統，您嘗試透過網路存取。  

6.  下列螢幕擷取畫面會顯示應該會出現錯誤訊息。  

    ![安全的本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認 「 拒絕登入以批次工作 」 GPO 設定  
從任何成員伺服器或工作站 GPO 變更的影響，在本機登入。  

###### <a name="create-a-batch-file"></a>建立批次檔  

1.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

2.  中**搜尋**方塊中，輸入**記事本**，然後按一下**記事本**。  

3.  在 **記事本**，型別**dir c:**。  

4.  按一下 **檔案**，然後按一下**另存新檔**。  

5.  在 **檔名**方塊中，輸入 **<Filename>.bat** (其中<Filename>是新的批次檔的名稱)。  

###### <a name="schedule-a-task"></a>排程的工作  

1.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

2.  在 **搜尋**方塊中輸入 工作排程器，然後按一下**工作排程器**。  

    > [!NOTE]  
    > 在電腦上執行 Windows 8 中，在**搜尋**方塊中，輸入**排程工作**，然後按一下**排程工作**。  

3.  按一下 **動作**，然後按一下**建立工作**。  

4.  在 [**建立工作**] 對話方塊中，輸入**<Task Name>** (其中<Task Name>是新工作的名稱)。  

5.  按一下 **動作**索引標籤，然後按一下**新增**。  

6.  在 **動作**欄位中，按一下**啟動程式**。  

7.  在 **程式或指令碼**欄位中，按一下**瀏覽**，找出並選取 批次檔中建立**建立批次檔**區段，然後按一下 **開啟**.  

8.  按一下 [確定] 。  

9. 按一下 [一般] 索引標籤。  

10. 在 **安全性選項**欄位中，按一下**變更使用者或群組**。  

11. 輸入系統的本機系統管理員帳戶的名稱，再按一下**檢查名稱**，然後按一下**確定**。  

12. 選取 **不論使用者登入與否均執行**並**不要儲存密碼**。 工作將只包含本機電腦資源的存取權。  

13. 按一下 [確定] 。  

14. 應該會出現一個對話方塊，要求的使用者帳戶的認證來執行工作。  

15. 輸入認證之後，按一下**確定**。  

16. 應該會出現如下所示的對話方塊。  

    ![安全的本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)  

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認 「 拒絕登入為服務 」 GPO 設定  

1.  從任何成員伺服器或工作站 GPO 變更的影響，在本機登入。  

2.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

3.  在 **搜尋**方塊中，輸入**服務**，然後按一下**服務**。  

4.  找出並按兩下**列印多工緩衝處理器**。  

5.  单击 **“登录”** 选项卡。  

6.  在 **身分登入**欄位中，按一下**此帳戶**。  

7.  按一下 **瀏覽**，輸入系統的本機系統管理員帳戶，按一下**檢查名稱**，然後按一下**確定**。  

8.  在 **密碼**並**確認密碼**欄位中，輸入所選的帳戶的密碼，然後按一下**確定**。  

9. 按一下 **確定**三次。  

10. 以滑鼠右鍵按一下**列印多工緩衝處理器**然後按一下**重新啟動**。  

11. 重新啟動服務時，應該會出現如下所示的對話方塊。  

    ![安全的本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)  

###### <a name="revert-changes-to-the-printer-spooler-service"></a>還原的列印多工緩衝處理器服務的變更  

1.  從任何成員伺服器或工作站 GPO 變更的影響，在本機登入。  

2.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

3.  在 **搜尋**方塊中，輸入**服務**，然後按一下**服務**。  

4.  找出並按兩下**列印多工緩衝處理器**。  

5.  单击 **“登录”** 选项卡。  

6.  在 **身分登入**： 欄位中，選取**本機 Systemaccount**，然後按一下**確定**。  

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>確認 「 拒絕透過登入遠端桌面服務 」 GPO 設定  

1.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

2.  在 **搜尋**方塊中，輸入**遠端桌面連線**，然後按一下**遠端桌面連線**。  

3.  在 **電腦**欄位中，輸入您想要連接到，然後按一下 電腦名稱**Connect**。 （您也可以輸入的 IP 位址，而非電腦名稱）。  

4.  出現提示時，提供認證的系統的當地**系統管理員**帳戶。  

5.  應該會出現如下所示的對話方塊。  

    ![安全的本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)  

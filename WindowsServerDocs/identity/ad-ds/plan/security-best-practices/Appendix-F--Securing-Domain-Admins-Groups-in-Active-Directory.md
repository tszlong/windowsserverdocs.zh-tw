---
ms.assetid: 017b88a6-f29b-4787-99b6-b5c8eaf8c3df
title: 附錄 F-保護 Active Directory 中的 Domain Admins 群組
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 4f453aa9f076b0272821849840106dae0c52fbbc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408697"
---
# <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>附錄 F︰保護 Active Directory 中的 Domain Admins 群組

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>附錄 F︰保護 Active Directory 中的 Domain Admins 群組  
就像 Enterprise Admins （EA）群組一樣，只有在組建或嚴重損壞修復的情況下，才需要 Domain Admins （DA）群組中的成員資格。 在 DA 群組中應該不會有每日的使用者帳戶，但網域的內建系統管理員帳戶除外，如[Active Directory 中的內建系統管理員帳戶的保護](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)中所述。  

根據預設，網域系統管理員會在其各自網域中的所有成員伺服器和工作站上，群組的成員。 此預設的嵌套不應針對支援性和嚴重損壞修復目的進行修改。 如果網域系統管理員已從成員伺服器上的本機 Administrators 群組中移除，則該群組應新增至每個成員伺服器上的 Administrators 群組和網域中的工作站。 每個網域的 Domain Admins 群組應受到保護，如下列逐步指示所述。  

針對樹系中每個網域的 Domain Admins 群組：  

1.  移除群組中的所有成員，但網域內建的系統管理員帳戶可能例外，前提是它已受到保護，如 Active Directory 中的內[建系統管理員帳戶的保護](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)中所述。  

2.  在連結到包含每個網域中成員伺服器和工作站之 Ou 的 Gpo 中，您應該將 DA 群組新增至電腦設定 \ \windows 設定 \ 許可權 \ 使用者**許可證指派**中的下列使用者權限：  

    -   拒絕從網路存取這台電腦  

    -   拒絕以批次工作登入  

    -   拒絕以服務方式登入  

    -   拒絕本機登入  

    -   拒絕透過遠端桌面服務使用者權限登入  

3.  如果對 Domain Admins 群組的內容或成員資格進行任何修改，則應該將審核設定為傳送警示。  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-domain-admins-group"></a>從 Domain Admins 群組移除所有成員的逐步指示  

1.  在**伺服器管理員**中，按一下 [**工具**]，然後按一下 [ **Active Directory 使用者和電腦**]。  

2.  若要從 DA 群組移除所有成員，請執行下列步驟：  

    1.  按兩下 [ **Domain Admins** ] 群組，然後按一下 [**成員**] 索引標籤。  

        ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_62.gif)  

    2.  選取群組的成員，按一下 [**移除**]，按一下 **[是]** ，然後按一下 **[確定]** 。  

3.  重複步驟2，直到所有的 DA 群組成員都已移除為止。  

#### <a name="step-by-step-instructions-to-secure-domain-admins-in-active-directory"></a>保護 Active Directory 中的網域系統管理員的逐步指示  

1.  在**伺服器管理員**中，按一下 [**工具**]，然後按一下 [**群組原則管理**]。  

2.  在主控台樹中，展開 \<樹系\>\\網域\\\<網域\>，然後**群組原則 物件** （其中 \<樹系\> 是樹系的名稱，\<網域\> 是您要設定群組原則的網功能變數名稱稱）。  

3.  在主控台樹中，以滑鼠右鍵按一下 [**群組原則物件**]，然後按一下 [**新增**]。  

    ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_63.gif)  

4.  在 **新增 GPO**  對話方塊中，輸入 \<GPO 名稱\>，然後按一下**確定** （其中 \<GPO 名稱\> 是此 GPO 的名稱）。  

    ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_64.gif)  

5.  在詳細資料窗格中，以滑鼠右鍵按一下 [\<GPO 名稱\>]，然後按一下 [**編輯**]。  

6.  流覽至 [**電腦設定 \ \windows] [設置 \ 本機原則**]，然後按一下 [**使用者權限指派**]。  

    ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_65.gif)  

7.  藉由執行下列動作，設定使用者權限以防止 Domain Admins 群組的成員透過網路存取成員伺服器和工作站：  

    1.  按兩下 **[拒絕從網路存取這部電腦**]，然後選取 [**定義這些原則設定**]。  

    2.  按一下 [**新增使用者或群組**]，然後按一下 **[流覽]** 。  

    3.  輸入**Domain Admins**，按一下 [**檢查名稱**]，然後按一下 **[確定]** 。  

        ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_66.gif)  

    4.  按一下 **[確定]** ，然後再按一次 **[確定]** 。  

8.  藉由執行下列動作，設定使用者權限以防止 DA 群組的成員以批次工作登入：  

    1.  按兩下 [**拒絕以批次工作登**入]，然後選取 [**定義這些原則設定**]。  

    2.  按一下 [**新增使用者或群組**]，然後按一下 **[流覽]** 。  

    3.  輸入**Domain Admins**，按一下 [**檢查名稱**]，然後按一下 **[確定]** 。  

        ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_67.gif)  

    4.  按一下 **[確定]** ，然後再按一次 **[確定]** 。  

9. 藉由執行下列動作，設定使用者權限以防止 DA 群組的成員以服務方式登入：  

    1.  按兩下 [**拒絕以服務方式登**入]，然後選取 [**定義這些原則設定**]。  

    2.  按一下 [**新增使用者或群組**]，然後按一下 **[流覽]** 。  

    3.  輸入**Domain Admins**，按一下 [**檢查名稱**]，然後按一下 **[確定]** 。  

        ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_68.gif)  

    4.  按一下 **[確定]** ，然後再按一次 **[確定]** 。  

10. 藉由執行下列動作，設定使用者權限以防止 Domain Admins 群組的成員在本機登入成員伺服器和工作站：  

    1.  按兩下 [**拒絕本機登**入]，然後選取 [**定義這些原則設定**]。  

    2.  按一下 [**新增使用者或群組**]，然後按一下 **[流覽]** 。  

    3.  輸入**Domain Admins**，按一下 [**檢查名稱**]，然後按一下 **[確定]** 。  

        ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_69.gif)  

    4.  按一下 **[確定]** ，然後再按一次 **[確定]** 。  

11. 藉由執行下列動作，設定使用者權限以防止 Domain Admins 群組的成員透過遠端桌面服務存取成員伺服器和工作站：  

    1.  按兩下 [**拒絕透過遠端桌面服務登**入]，然後選取 [**定義這些原則設定**]。  

    2.  按一下 [**新增使用者或群組**]，然後按一下 **[流覽]** 。  

    3.  輸入**Domain Admins**，按一下 [**檢查名稱**]，然後按一下 **[確定]** 。  

        ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_70.gif)  

    4.  按一下 **[確定]** ，然後再按一次 **[確定]** 。  

12. 若要結束**群組原則管理編輯器**，**請按一下 [** 檔案]，然後按一下 [結束 **]。**  

13. 在群組原則管理 中，執行下列動作，將 GPO 連結到成員伺服器和工作站 Ou：  

    1.  流覽至 \<樹系\>\Domains\\\<網域\> （其中 \<樹系\> 是樹系的名稱，而 \<網域\> 是您要設定群組原則的網功能變數名稱稱）。  

    2.  以滑鼠右鍵按一下要套用 GPO 的 OU，然後按一下 [**連結到現有的 gpo**]。  

        ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_71.gif)  

    3.  選取您剛才建立的 GPO，然後按一下 **[確定]** 。  

        ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_72.gif)  

    4.  建立包含工作站之所有其他 Ou 的連結。  

    5.  建立包含成員伺服器的所有其他 Ou 的連結。  

        > [!IMPORTANT]  
        > 如果使用跳躍伺服器來管理網域控制站和 Active Directory，請確定跳躍伺服器位於未連結此 Gpo 的 OU 中。  

#### <a name="verification-steps"></a>驗證步驟  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>確認「拒絕從網路存取這部電腦」的 GPO 設定  
從任何不受 GPO 影響的成員伺服器或工作站（例如「跳躍伺服器」），嘗試透過受 GPO 所影響的網路存取成員伺服器或工作站。 若要確認 GPO 設定，請嘗試使用**NET USE**命令來對應系統磁片磁碟機。  

1.  使用屬於 Domain Admins 群組成員的帳戶在本機登入。  

2.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。  

3.  在**搜尋**方塊中，輸入**命令提示**字元，以滑鼠右鍵按一下 [**命令提示**字元]，然後按一下 [以**系統管理員身分執行**] 以開啟提升許可權的命令提示字元。  

4.  當系統提示您核准提高許可權時，按一下 **[是]** 。  

    ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_73.gif)  

5.  在 [**命令提示**字元] 視窗中，輸入**net use \\\\\<伺服器名稱\>\c $** ，其中 \<伺服器名稱\> 是您嘗試透過網路存取的成員伺服器或工作站的名稱。  

6.  下列螢幕擷取畫面顯示應該顯示的錯誤訊息。  

    ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_74.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認 [拒絕以批次工作登入] GPO 設定  

從任何受 GPO 影響的成員伺服器或工作站變更，在本機登入。  

###### <a name="create-a-batch-file"></a>建立批次檔  

1.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。  

2.  在**搜尋**方塊中，輸入**notepad**，然後按一下 [**記事本**]。  

3.  在 [**記事本**] 中，輸入**dir c：** 。  

4.  按一下 [檔案 **]，然後按一下 [** **另存**新檔]。  

5.  在 [**檔案名**] 欄位中，輸入 **\<filename\>.bat** （其中 \<Filename\> 是新批次檔的名稱）。  

###### <a name="schedule-a-task"></a>排程工作  

1.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。  

2.  在 [**搜尋**] 方塊中 **，輸入 [** 工作排程器]，然後按一下 [**工作排程器**]。  

    > [!NOTE]  
    > 在執行 Windows 8 的電腦上，于 [**搜尋**] 方塊中輸入 [**排程**工作]，然後按一下 [**排程**工作]。  

3.  在 [**工作排程器**] 功能表列中，按一下 [**動作**]，然後按一下 [**建立**工作]。  

4.  在 [**建立**工作] 對話方塊中，輸入 **\<工作名稱\>** （其中 \<[工作名稱]\> 是新工作的名稱）。  

5.  按一下 [**動作**] 索引標籤，然後按一下 [**新增**]。  

6.  在 [**動作**] 欄位中，選取 [**啟動程式**]。  

7.  在 [**程式/腳本**] 底下，按一下 **[流覽]** ，找出並選取建立**批次檔**一節中建立的批次檔，然後按一下 [**開啟**]。  

8.  按一下 **\[確定\]** 。  

9. 按一下 [一般] 索引標籤。  

10. 在 [**安全性**選項] 底下，按一下 [**變更使用者或群組**]。  

11. 輸入屬於 Domain Admins 群組成員的帳戶名稱，按一下 [**檢查名稱**]，然後按一下 **[確定]** 。  

12. 選取 **[執行]，無論使用者是否登入**，並選取 [不要**儲存密碼**]。 此工作只會存取本機電腦資源。  

13. 按一下 **\[確定\]** 。  

14. 對話方塊應該會出現，要求使用者帳號憑證才能執行工作。  

15. 輸入認證之後，請按一下 **[確定]** 。  

16. 應該會出現類似下列的對話方塊。  

    ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_75.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認「拒絕以服務方式登入」的 GPO 設定  

1.  從任何受 GPO 影響的成員伺服器或工作站變更，在本機登入。  

2.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。  

3.  在 [**搜尋**] 方塊中，輸入**服務**，然後按一下 [**服務**]。  

4.  找出並按兩下 [**列印多工緩衝處理器**]。  

5.  单击 **“登录”** 选项卡。  

6.  在 [**登**入身分] 底下，選取 [**此帳戶**] 選項。  

7.  按一下 **[流覽]** ，輸入屬於 Domain Admins 群組成員的帳戶名稱，按一下 [**檢查名稱**]，然後按一下 **[確定]** 。  

8.  在 [**密碼**] 和 [**確認密碼**] 下，輸入選取帳戶的密碼，然後按一下 **[確定]** 。  

9. 再按三次 **[確定]** 。  

10. 以滑鼠右鍵按一下 [**列印多工緩衝處理器**] 並按一下 [**重新開機**]。  

11. 重新開機服務時，應該會出現類似下列的對話方塊。  

    ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_76.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>還原對印表機多工緩衝處理程式服務所做的變更  

1.  從任何受 GPO 影響的成員伺服器或工作站變更，在本機登入。  

2.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。  

3.  在 [**搜尋**] 方塊中，輸入**服務**，然後按一下 [**服務**]。  

4.  找出並按兩下 [**列印多工緩衝處理器**]。  

5.  单击 **“登录”** 选项卡。  

6.  在 [**登**入身分] 底下，選取 [**本機系統**] 帳戶，然後按一下 **[確定]** 。  

##### <a name="verify-deny-log-on-locally-gpo-settings"></a>確認 [拒絕本機登入] GPO 設定  

1.  從任何受 GPO 影響的成員伺服器或工作站變更，嘗試使用屬於 Domain Admins 群組成員的帳戶在本機登入。 應該會出現類似下列的對話方塊。  

    ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_77.gif)  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>確認「拒絕透過遠端桌面服務登入」 GPO 設定    
1.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。  

2.  在 [**搜尋**] 方塊中，輸入 [**遠端桌面**連線]，然後按一下 [**遠端桌面連線**]。  

3.  在 [**電腦**] 欄位中，輸入您要連線的電腦名稱稱，然後按一下 **[連線]** 。 （您也可以輸入 IP 位址，而不是電腦名稱稱。）  

4.  出現提示時，提供屬於 Domain Admins 群組成員之帳戶的認證。  

5.  應該會出現類似下列的對話方塊。  

    ![保護網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_78.gif)  

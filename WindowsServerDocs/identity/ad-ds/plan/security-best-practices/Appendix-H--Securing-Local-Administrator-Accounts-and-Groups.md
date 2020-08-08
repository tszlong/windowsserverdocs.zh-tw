---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: 附錄 H-保護本機系統管理員帳戶和群組
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c3d9e6bd810b0d0c3d3d6aac310b3ff058b3e018
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963246"
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>附錄 H︰保護本機 Administrators 帳戶和群組

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>附錄 H︰保護本機 Administrators 帳戶和群組
在目前處於主流支援的所有 Windows 版本上，預設會停用本機系統管理員帳戶，讓帳戶無法用於傳遞雜湊和其他認證竊取攻擊。 不過，在包含舊版作業系統或已啟用本機系統管理員帳戶的環境中，可以使用這些帳戶，如先前所述，以在成員伺服器和工作站之間傳播危害。 每個本機系統管理員帳戶和群組都應該受到保護，如下列逐步指示所述。

如需有關保護內建系統管理員 (BA) 群組之考慮的詳細資訊，請參閱[執行最低許可權的管理模型](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)。

#### <a name="controls-for-local-administrator-accounts"></a>本機系統管理員帳戶的控制項
對於樹系中每個網域的本機系統管理員帳戶，您應該設定下列設定：

-   設定 Gpo 來限制網域的系統管理員帳戶在已加入網域的系統上的使用
    -   在您建立的一或多個 Gpo 中，以及連結至每個網域中的工作站和成員伺服器 Ou，將系統管理員帳戶新增至**電腦設定 \ \windows 設定 \**使用者權限 \ 許可權指派中的下列使用者權限：

        -   拒絕從網路存取這部電腦

        -   拒絕以批次工作登入

        -   拒絕以服務方式登入

        -   拒絕透過遠端桌面服務登入

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>保護本機系統管理員群組的逐步指示

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>設定 Gpo 來限制已加入網域之系統上的系統管理員帳戶

1.  在**伺服器管理員**中，按一下 [**工具**]，然後按一下 [**群組原則管理**]。

2.  在主控台樹中，展開 [ <Forest> \Domains] \\ <Domain> ，然後**群組原則物件**] (其中 <Forest> 是樹系的名稱，而 <Domain> 是您要設定群組原則) 的功能變數名稱。

3.  在主控台樹中，以滑鼠右鍵按一下 [**群組原則物件**]，然後按一下 [**新增**]。

    ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)

4.  在 [**新增 GPO** ] 對話方塊中，輸入 **<GPO Name>** ，然後按一下 **[確定]** (其中 <GPO Name> 是此 GPO) 的名稱。

    ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)

5.  在詳細資料窗格中，按一下滑鼠右鍵 **<GPO Name>** ，然後按一下 [**編輯**]。

6.  流覽至 [**電腦設定 \ \windows] [設置 \ 本機原則**]，然後按一下 [**使用者權限指派**]。

    ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)

7.  設定使用者權限，以防止本機系統管理員帳戶透過網路存取成員伺服器和工作站，方法是執行下列動作：

    1.  按兩下 **[拒絕從網路存取這部電腦**]，然後選取 [**定義這些原則設定**]。

    2.  按一下 [**新增使用者或群組**]，輸入本機系統管理員帳戶的使用者名稱，然後按一下 **[確定]**。 此使用者名稱將會是**Administrator**，這是安裝 Windows 時的預設值。

        ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)

    3.  按一下 [確定]。

        > [!IMPORTANT]
        > 當您將系統管理員帳戶新增至這些設定時，您可以指定是要依照如何為帳戶加上標籤來設定本機系統管理員帳戶或網域系統管理員帳戶。 例如，若要將 TAILSPINTOYS 網域的系統管理員帳戶新增至這些拒絕許可權，您可以流覽至 TAILSPINTOYS 網域的系統管理員帳戶，這會顯示為 TAILSPINTOYS\Administrator。 如果您在群組原則物件編輯器的這些使用者權限設定中輸入**系統管理員**，您將會限制套用 GPO 的每部電腦上的本機系統管理員帳戶，如先前所述。

8.  設定使用者權限，以防止本機系統管理員帳戶以批次工作登入，方法是執行下列動作：

    1.  按兩下 [**拒絕以批次工作登**入]，然後選取 [**定義這些原則設定**]。

    2.  按一下 [**新增使用者或群組**]，輸入本機系統管理員帳戶的使用者名稱，然後按一下 **[確定]**。 此使用者名稱將會是**Administrator**，這是安裝 Windows 時的預設值。

        ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)

    3.  按一下 [確定]  。

        > [!IMPORTANT]
        > 當您將系統管理員帳戶新增至這些設定時，您可以指定是要依照如何為帳戶加上標籤來設定本機系統管理員帳戶或網域系統管理員帳戶。 例如，若要將 TAILSPINTOYS 網域的系統管理員帳戶新增至這些拒絕許可權，您可以流覽至 TAILSPINTOYS 網域的系統管理員帳戶，這會顯示為 TAILSPINTOYS\Administrator。 如果您在群組原則物件編輯器的這些使用者權限設定中輸入**系統管理員**，您將會限制套用 GPO 的每部電腦上的本機系統管理員帳戶，如先前所述。

9. 設定使用者權限，以防止本機系統管理員帳戶以服務的身分登入，方法是執行下列動作：

    1.  按兩下 [**拒絕以服務方式登**入]，然後選取 [**定義這些原則設定**]。

    2.  按一下 [**新增使用者或群組**]，輸入本機系統管理員帳戶的使用者名稱，然後按一下 **[確定]**。 此使用者名稱將會是**Administrator**，這是安裝 Windows 時的預設值。

        ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)

    3.  按一下 [確定]  。

        > [!IMPORTANT]
        > 當您將系統管理員帳戶新增至這些設定時，您可以指定是要依照如何為帳戶加上標籤來設定本機系統管理員帳戶或網域系統管理員帳戶。 例如，若要將 TAILSPINTOYS 網域的系統管理員帳戶新增至這些拒絕許可權，您可以流覽至 TAILSPINTOYS 網域的系統管理員帳戶，這會顯示為 TAILSPINTOYS\Administrator。 如果您在群組原則物件編輯器的這些使用者權限設定中輸入**系統管理員**，您將會限制套用 GPO 的每部電腦上的本機系統管理員帳戶，如先前所述。

10. 藉由執行下列動作來設定使用者權限，以防止本機系統管理員帳戶透過遠端桌面服務來存取成員伺服器和工作站：

    1.  按兩下 [**拒絕透過遠端桌面服務登**入]，然後選取 [**定義這些原則設定**]。

    2.  按一下 [**新增使用者或群組**]，輸入本機系統管理員帳戶的使用者名稱，然後按一下 **[確定]**。 此使用者名稱將會是**Administrator**，這是安裝 Windows 時的預設值。

        ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)

    3.  按一下 [確定]  。

        > [!IMPORTANT]
        > 當您將系統管理員帳戶新增至這些設定時，您可以指定是要依照如何為帳戶加上標籤來設定本機系統管理員帳戶或網域系統管理員帳戶。 例如，若要將 TAILSPINTOYS 網域的系統管理員帳戶新增至這些拒絕許可權，您可以流覽至 TAILSPINTOYS 網域的系統管理員帳戶，這會顯示為 TAILSPINTOYS\Administrator。 如果您在群組原則物件編輯器的這些使用者權限設定中輸入**系統管理員**，您將會限制套用 GPO 的每部電腦上的本機系統管理員帳戶，如先前所述。

11. 若要結束**群組原則管理編輯器**，**請按一下 [** 檔案]，然後按一下 [結束 **]。**

12. 在**群組原則管理**] 中，執行下列動作，將 GPO 連結到成員伺服器和工作站 ou：

    1.  流覽至 [ <Forest> \Domains (]， \\ <Domain> 其中 <Forest> 是樹系的名稱，而 <Domain> 是您要設定群組原則) 的網功能變數名稱稱。

    2.  以滑鼠右鍵按一下要套用 GPO 的 OU，然後按一下 [**連結到現有的 gpo**]。

        ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)

    3.  選取您建立的 GPO，然後按一下 **[確定]**。

        ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)

    4.  建立包含工作站之所有其他 Ou 的連結。

    5.  建立包含成員伺服器的所有其他 Ou 的連結。

#### <a name="verification-steps"></a>驗證步驟

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>確認「拒絕從網路存取這部電腦」的 GPO 設定

從任何不受 GPO 影響的成員伺服器或工作站 (例如跳躍伺服器) ，嘗試透過受 GPO 影響的網路存取成員伺服器或工作站。 若要確認 GPO 設定，請嘗試使用**NET USE**命令來對應系統磁片磁碟機。

1.  本機登入不受 GPO 變更影響的任何成員伺服器或工作站。

2.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。

3.  在**搜尋**方塊中，輸入**命令提示**字元，以滑鼠右鍵按一下 [**命令提示**字元]，然後按一下 [以**系統管理員身分執行**] 以開啟提升許可權的命令提示字元。

4.  當系統提示您核准提高許可權時，按一下 **[是]**。

    ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)

5.  在 [**命令提示**字元] 視窗中，輸入**net use \\ \\ <Server Name> \c $/user： <Server Name> \Administrator**，其中 <Server Name> 是您嘗試透過網路存取的成員伺服器或工作站的名稱。

    > [!NOTE]
    > 本機系統管理員認證必須來自您嘗試透過網路存取的相同系統。

6.  下列螢幕擷取畫面顯示應該顯示的錯誤訊息。

    ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認 [拒絕以批次工作登入] GPO 設定
從任何受 GPO 影響的成員伺服器或工作站變更，在本機登入。

###### <a name="create-a-batch-file"></a>建立批次檔

1.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。

2.  在**搜尋**方塊中，輸入**notepad**，然後按一下 [**記事本**]。

3.  在 [**記事本**] 中，輸入**dir c：**。

4.  按一下 [檔案 **]，然後按一下 [****另存**新檔]。

5.  在 [**檔案名**] 方塊中，輸入** <Filename> .bat** (其中 <Filename> 是新批次檔的名稱) 。

###### <a name="schedule-a-task"></a>排程工作

1.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。

2.  在 [**搜尋**] 方塊中，輸入 [工作排程器]，然後按一下 [**工作排程器**]。

    > [!NOTE]
    > 在執行 Windows 8 的電腦上，于 [**搜尋**] 方塊中輸入 [**排程**工作]，然後按一下 [**排程**工作]。

3.  按一下 [**動作**]，然後按一下 [**建立**工作]。

4.  在 [**建立**工作] 對話方塊中，輸入 **<Task Name>** (，其中 <Task Name> 是新工作) 的名稱。

5.  按一下 [**動作**] 索引標籤，然後按一下 [**新增**]。

6.  在 [**動作**] 欄位中，按一下 [**啟動程式**]。

7.  在 [**程式/腳本**] 欄位中，按一下 **[流覽]**，找出並選取建立**批次檔**一節中建立的批次檔，然後按一下 [**開啟**]。

8.  按一下 [確定]  。

9. 按一下 [General] \(一般\) 索引標籤。

10. 在 [**安全性選項**] 欄位中，按一下 [**變更使用者或群組**]。

11. 輸入系統的本機系統管理員帳戶名稱，按一下 [**檢查名稱**]，然後按一下 **[確定]**。

12. 選取 **[執行] 表示使用者是否登入**，而且不會**儲存密碼**。 此工作只會存取本機電腦資源。

13. 按一下 [確定]  。

14. 對話方塊應該會出現，要求使用者帳號憑證才能執行工作。

15. 輸入認證之後，請按一下 **[確定]**。

16. 應該會出現類似下列的對話方塊。

    ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認「拒絕以服務方式登入」的 GPO 設定

1.  從任何受 GPO 影響的成員伺服器或工作站變更，在本機登入。

2.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。

3.  在 [**搜尋**] 方塊中，輸入**服務**，然後按一下 [**服務**]。

4.  找出並按兩下 [**列印多工緩衝處理器**]。

5.  单击 **“登录”** 选项卡。

6.  在 [**登**入身分] 欄位中，按一下 [**這個帳戶**]。

7.  按一下 **[流覽]**，輸入系統的本機系統管理員帳戶，按一下 [**檢查名稱**]，然後按一下 **[確定]**。

8.  在 [**密碼**] 和 [**確認密碼**] 欄位中，輸入所選帳戶的密碼，然後按一下 **[確定]**。

9. 再按三次 **[確定]** 。

10. 以滑鼠右鍵按一下 [**列印多工緩衝處理器**] 並按一下 [**重新開機**]。

11. 重新開機服務時，應該會出現類似下列的對話方塊。

    ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)

###### <a name="revert-changes-to-the-printer-spooler-service"></a>還原對印表機多工緩衝處理程式服務所做的變更

1.  從任何受 GPO 影響的成員伺服器或工作站變更，在本機登入。

2.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。

3.  在 [**搜尋**] 方塊中，輸入**服務**，然後按一下 [**服務**]。

4.  找出並按兩下 [**列印多工緩衝處理器**]。

5.  单击 **“登录”** 选项卡。

6.  在 [**登**入身分：] 欄位中，選取 [**本機 Systemaccount**]，然後按一下 **[確定]**。

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>確認「拒絕透過遠端桌面服務登入」 GPO 設定

1.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。

2.  在 [**搜尋**] 方塊中，輸入 [**遠端桌面**連線]，然後按一下 [**遠端桌面連線**]。

3.  在 [**電腦**] 欄位中，輸入您要連線的電腦名稱稱，然後按一下 **[連線]**。  (您也可以輸入 IP 位址，而不是電腦名稱稱。 ) 

4.  出現提示時，請提供系統本機**管理員**帳戶的認證。

5.  應該會出現類似下列的對話方塊。

    ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)

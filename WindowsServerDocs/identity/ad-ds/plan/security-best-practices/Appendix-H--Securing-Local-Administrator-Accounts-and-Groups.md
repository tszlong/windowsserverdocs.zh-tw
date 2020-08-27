---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: 附錄 H-保護本機系統管理員帳戶和群組
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 2b8e5de1bd45fba443ce2d91105954df1d049a14
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88938398"
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>附錄 H︰保護本機 Administrators 帳戶和群組

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>附錄 H︰保護本機 Administrators 帳戶和群組
在所有目前的 Windows 版本上，預設會停用本機系統管理員帳戶，讓帳戶無法用於傳遞雜湊和其他認證竊取攻擊。 不過，在包含舊版作業系統或啟用本機系統管理員帳戶的環境中，這些帳戶可以如先前所述，在成員伺服器和工作站之間傳播折衷。 每個本機系統管理員帳戶和群組都應受到保護，如後續的逐步指示所述。

如需有關保護內建系統管理員 (BA) 群組之考慮的詳細資訊，請參閱 [執行最低許可權的系統管理模型](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)。

#### <a name="controls-for-local-administrator-accounts"></a>本機系統管理員帳戶的控制項
針對樹系中每個網域的本機系統管理員帳戶，您應該設定下列設定：

-   設定 Gpo 以限制網域已加入系統的系統管理員帳戶使用
    -   在您建立並連結到每個網域中的工作站和成員伺服器 Ou 的一或多個 Gpo 中，將系統管理員帳戶新增至 **電腦設定 \windows 設定**設定 \ 使用者權限 \ 使用者權限指派中的下列使用者權限：

        -   拒絕從網路存取這部電腦

        -   拒絕以批次工作登入

        -   拒絕以服務方式登入

        -   拒絕透過遠端桌面服務登入

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>保護本機系統管理員群組的逐步指示

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>設定 Gpo 以限制已加入網域之系統的系統管理員帳戶

1.  在 **伺服器管理員**中，按一下 [ **工具**]，然後按一下 [ **群組原則管理**]。

2.  在主控台樹中，展開 [ <Forest> \Domains] \\ <Domain> ，然後**群組原則物件** (其中 <Forest> 是樹系的名稱，而 <Domain> 是您要設定群組原則) 的網功能變數名稱稱。

3.  在主控台樹中，以滑鼠右鍵按一下 **群組原則物件**]，然後按一下 [ **新增**]。

    ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)

4.  在 [ **新增 GPO** ] 對話方塊中，輸入 **<GPO Name>** ，然後按一下 **[確定]** (其中 <GPO Name> 是此 GPO) 的名稱。

    ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)

5.  在詳細資料窗格中，以滑鼠右鍵按一下 **<GPO Name>** ，然後按一下 [ **編輯**]。

6.  流覽至 [ **電腦設定 \windows 設定] \ 使用者設置 \ 本機原則**，然後按一下 [ **使用者權限指派**]。

    ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)

7.  設定使用者權限，以防止本機系統管理員帳戶透過網路存取成員伺服器和工作站，請執行下列動作：

    1.  按兩下 **[拒絕從網路存取這部電腦** ]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組**]，輸入本機系統管理員帳戶的使用者名稱，然後按一下 **[確定]**。 此使用者名稱將會是 [ **系統管理員**]，也就是 Windows 安裝時的預設值。

        ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)

    3.  按一下 [確定]。

        > [!IMPORTANT]
        > 當您將系統管理員帳戶新增到這些設定時，您會指定要設定本機系統管理員帳戶或網域系統管理員帳戶，方法是將帳戶加上標籤的方式。 例如，若要將 TAILSPINTOYS 網域的系統管理員帳戶新增到這些拒絕許可權，您可以流覽至 TAILSPINTOYS 網域的系統管理員帳戶，此帳戶會顯示為 TAILSPINTOYS\Administrator。 如果您在群組原則物件編輯器的這些使用者權限設定中輸入 **系統管理員** ，您將會限制套用 GPO 之每部電腦上的本機系統管理員帳戶，如先前所述。

8.  設定使用者權限，以防止本機系統管理員帳戶以批次工作登入，方法是執行下列動作：

    1.  按兩下 [ **拒絕以批次工作登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組**]，輸入本機系統管理員帳戶的使用者名稱，然後按一下 **[確定]**。 此使用者名稱將會是 [ **系統管理員**]，也就是 Windows 安裝時的預設值。

        ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)

    3.  按一下 [確定]。

        > [!IMPORTANT]
        > 當您將系統管理員帳戶新增至這些設定時，您會指定要設定本機系統管理員帳戶或網域系統管理員帳戶，方法是將帳戶加上標籤的方式。 例如，若要將 TAILSPINTOYS 網域的系統管理員帳戶新增到這些拒絕許可權，您可以流覽至 TAILSPINTOYS 網域的系統管理員帳戶，此帳戶會顯示為 TAILSPINTOYS\Administrator。 如果您在群組原則物件編輯器的這些使用者權限設定中輸入 **系統管理員** ，您將會限制套用 GPO 之每部電腦上的本機系統管理員帳戶，如先前所述。

9. 設定使用者權限，以防止本機系統管理員帳戶以服務的方式登入，方法是執行下列動作：

    1.  按兩下 [ **拒絕以服務方式登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組**]，輸入本機系統管理員帳戶的使用者名稱，然後按一下 **[確定]**。 此使用者名稱將會是 [ **系統管理員**]，也就是 Windows 安裝時的預設值。

        ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)

    3.  按一下 [確定]。

        > [!IMPORTANT]
        > 當您將系統管理員帳戶新增至這些設定時，您會指定要設定本機系統管理員帳戶或網域系統管理員帳戶，方法是將帳戶加上標籤的方式。 例如，若要將 TAILSPINTOYS 網域的系統管理員帳戶新增到這些拒絕許可權，您可以流覽至 TAILSPINTOYS 網域的系統管理員帳戶，此帳戶會顯示為 TAILSPINTOYS\Administrator。 如果您在群組原則物件編輯器的這些使用者權限設定中輸入 **系統管理員** ，您將會限制套用 GPO 之每部電腦上的本機系統管理員帳戶，如先前所述。

10. 藉由執行下列動作，設定使用者權限以防止本機系統管理員帳戶透過遠端桌面服務存取成員伺服器和工作站：

    1.  按兩下 [ **拒絕透過遠端桌面服務登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組**]，輸入本機系統管理員帳戶的使用者名稱，然後按一下 **[確定]**。 此使用者名稱將會是 [ **系統管理員**]，也就是 Windows 安裝時的預設值。

        ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)

    3.  按一下 [確定]。

        > [!IMPORTANT]
        > 當您將系統管理員帳戶新增至這些設定時，您會指定要設定本機系統管理員帳戶或網域系統管理員帳戶，方法是將帳戶加上標籤的方式。 例如，若要將 TAILSPINTOYS 網域的系統管理員帳戶新增到這些拒絕許可權，您可以流覽至 TAILSPINTOYS 網域的系統管理員帳戶，此帳戶會顯示為 TAILSPINTOYS\Administrator。 如果您在群組原則物件編輯器的這些使用者權限設定中輸入 **系統管理員** ，您將會限制套用 GPO 之每部電腦上的本機系統管理員帳戶，如先前所述。

11. 若要結束**群組原則管理編輯器**，**請按一下 [** 檔案]，然後按一下 [結束 **]。**

12. 在 **群組原則管理**] 中，執行下列動作將 GPO 連結到成員伺服器和工作站 ou：

    1.  流覽至 <Forest> \Domains \\ <Domain> (其中 <Forest> 是樹系的名稱，而 <Domain> 是您要設定群組原則) 之網域的名稱。

    2.  以滑鼠右鍵按一下 GPO 將套用的 OU，然後按一下 [ **連結現有的 gpo**]。

        ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)

    3.  選取您所建立的 GPO，然後按一下 **[確定]**。

        ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)

    4.  建立包含工作站之所有其他 Ou 的連結。

    5.  建立包含成員伺服器的所有其他 Ou 的連結。

#### <a name="verification-steps"></a>驗證步驟

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>確認「拒絕從網路存取這部電腦」 GPO 設定

從任何不受 GPO 影響的成員伺服器或工作站 (例如跳躍伺服器) ，嘗試透過受 GPO 變更影響的網路來存取成員伺服器或工作站。 若要確認 GPO 設定，請使用 **NET USE** 命令嘗試對應系統磁片磁碟機。

1.  在本機登入任何不受 GPO 變更影響的成員伺服器或工作站。

2.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

3.  在 [ **搜尋** ] 方塊中，輸入 **命令提示**字元，以滑鼠右鍵按一下 [ **命令提示**字元]，然後按一下 [以 **系統管理員身分執行** ] 來開啟提升許可權的命令提示字元。

4.  當系統提示您核准提高許可權時，請按一下 **[是]**。

    ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)

5.  在 [**命令提示**字元] 視窗中，輸入**net use \\ \\ <Server Name> \c $/user： <Server Name> \Administrator**，其中 <Server Name> 是您嘗試透過網路存取的成員伺服器或工作站的名稱。

    > [!NOTE]
    > 本機系統管理員認證必須來自您嘗試透過網路存取的相同系統。

6.  下列螢幕擷取畫面顯示應出現的錯誤訊息。

    ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認「拒絕以批次工作登入」 GPO 設定
從受 GPO 變更影響的任何成員伺服器或工作站，在本機登入。

###### <a name="create-a-batch-file"></a>建立批次檔

1.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

2.  在 [ **搜尋** ] 方塊中，輸入 [ **記事本**]，然後按一下 [ **記事本**]。

3.  在 [ **記事本**] 中，輸入 **dir c：**。

4.  按一下 [檔案]，**然後按一下 [****另存**新檔]。

5.  在 [**檔案名**] 方塊中，輸入** <Filename> .bat** (其中 <Filename> 是) 的新批次檔的名稱。

###### <a name="schedule-a-task"></a>排程工作

1.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

2.  在 [ **搜尋** ] 方塊中，輸入 [工作排程器]，然後按一下 [ **工作排程器**]。

    > [!NOTE]
    > 在執行 Windows 8 的電腦上，于 [ **搜尋** ] 方塊中輸入 **排程**工作，然後按一下 [ **排程**工作]。

3.  按一下 [ **動作**]，然後按一下 [ **建立**工作]。

4.  在 [ **建立** 工作] 對話方塊中，輸入 **<Task Name>** (其中 <Task Name> 是新工作) 的名稱。

5.  按一下 [ **動作** ] 索引標籤，然後按一下 [ **新增**]。

6.  在 [ **動作** ] 欄位中，按一下 [ **啟動程式**]。

7.  在 [ **程式/腳本** ] 欄位中，按一下 **[流覽]**，找出並選取 [ **建立批次檔** ] 區段中建立的批次檔，然後按一下 [ **開啟**]。

8.  按一下 [確定]。

9. 按一下 [General] \(一般\) 索引標籤。

10. 在 [ **安全性選項** ] 欄位中，按一下 [ **變更使用者或群組**]。

11. 輸入系統本機系統管理員帳戶的名稱，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

12. 選取 **[執行]，不論使用者是否登入** ，並 **不要儲存密碼**。 此工作將只能存取本機電腦資源。

13. 按一下 [確定]。

14. 應該會出現一個對話方塊，要求使用者帳號憑證來執行工作。

15. 輸入認證之後，請按一下 **[確定]**。

16. 應該會出現類似下列的對話方塊。

    ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認「拒絕以服務方式登入」 GPO 設定

1.  從受 GPO 變更影響的任何成員伺服器或工作站，在本機登入。

2.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

3.  在 [ **搜尋** ] 方塊中，輸入 **服務**，然後按一下 [ **服務**]。

4.  找出並按兩下 [ **列印多工緩衝處理器**]。

5.  单击 **“登录”** 选项卡。

6.  在 [ **登入為** ] 欄位中，按一下 [ **這個帳戶**]。

7.  按一下 **[流覽]**，輸入系統的本機系統管理員帳戶，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

8.  在 [ **密碼** ] 和 [ **確認密碼** ] 欄位中，輸入選取的帳戶密碼，然後按一下 **[確定]**。

9. 再按三次 **[確定]** 。

10. 以滑鼠右鍵按一下 [ **列印多工緩衝處理器** ]，然後按一下 [ **重新開機**]

11. 當服務重新開機時，應該會出現類似下列的對話方塊。

    ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)

###### <a name="revert-changes-to-the-printer-spooler-service"></a>還原印表機多工緩衝處理器服務的變更

1.  從受 GPO 變更影響的任何成員伺服器或工作站，在本機登入。

2.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

3.  在 [ **搜尋** ] 方塊中，輸入 **服務**，然後按一下 [ **服務**]。

4.  找出並按兩下 [ **列印多工緩衝處理器**]。

5.  单击 **“登录”** 选项卡。

6.  在 [ **登入為**：] 欄位中，選取 [ **本機 Systemaccount**]，然後按一下 **[確定]**。

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>確認「拒絕登入遠端桌面服務」 GPO 設定

1.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

2.  在 [ **搜尋** ] 方塊中，輸入 **remote desktop connection**，然後按一下 [ **遠端桌面連線**]。

3.  在 [ **電腦** ] 欄位中，輸入您要連線的電腦名稱稱，然後按一下 [連線 **]**。  (您也可以輸入 IP 位址，而不是電腦名稱稱。 ) 

4.  出現提示時，請提供系統本機系統 **管理員** 帳戶的認證。

5.  應該會出現類似下列的對話方塊。

    ![保護本機系統管理員帳戶和群組](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)

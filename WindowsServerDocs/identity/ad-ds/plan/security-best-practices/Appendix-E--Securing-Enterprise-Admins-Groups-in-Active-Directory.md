---
description: 深入瞭解：附錄 E：保護 Active Directory 中的 Enterprise Admins 群組
ms.assetid: f643099e-f9c6-476f-9378-5a9228c39b33
title: 附錄 E-保護 Active Directory 中的 Enterprise Admins 群組
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 0abcc31dcf61ca66c79c9f6802ddd299ad92e47c
ms.sourcegitcommit: e2dadc9b0c227a489a945bbc531aca5e101f18cd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97801802"
---
# <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>附錄 E︰保護 Active Directory 中的 Enterprise Admins 群組

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>附錄 E︰保護 Active Directory 中的 Enterprise Admins 群組
企業系統管理員 (EA) 群組（位於樹系根域中）不應每天包含任何使用者，但根域的系統管理員帳戶可能例外，前提是它受到保護，如 [附錄 D：保護 Active Directory 中的 Built-In 系統管理員](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)帳戶所述。

依預設，Enterprise Admins 是樹系中每個網域的 Administrators 群組成員。 您不應該從每個網域中的系統管理員群組中移除 EA 群組，因為在發生樹系嚴重損壞修復案例的情況下，EA 許可權可能會是必要的。 樹系的 Enterprise Admins 群組應受到保護，如後續的逐步指示所述。

針對樹系中的 Enterprise Admins 群組：

1.  在連結到包含成員伺服器和每個網域中工作站之 Ou 的 Gpo 中，Enterprise Admins 群組應新增至電腦設定 \windows 設定設定 \ 使用者 \ 使用者 \ 使用者 **權力指派許可權指派** 中的下列使用者權限：

    -   拒絕從網路存取這部電腦

    -   拒絕以批次工作登入

    -   拒絕以服務方式登入

    -   拒絕本機登入

    -   拒絕透過遠端桌面服務登入

2.  如果對 Enterprise Admins 群組的屬性或成員資格進行任何修改，請設定 [審核] 以傳送警示。

### <a name="step-by-step-instructions-for-removing-all-members-from-the-enterprise-admins-group"></a>從 Enterprise Admins 群組移除所有成員的逐步指示

1.  在 **伺服器管理員** 中，按一下 [ **工具**]，然後按一下 [ **Active Directory 消費者和電腦**]。

2.  如果您不是管理樹系的根域，請在主控台樹中，以滑鼠右鍵按一下 <Domain> ，然後按一下 [ **變更網域** ] (其中 <Domain> 是您目前所管理之網域的名稱) 。

    ![醒目顯示 [變更網域] 功能表選項的螢幕擷取畫面。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_43.gif)

3.  在 [ **變更網域** ] 對話方塊中，按一下 [ **流覽]**，選取樹系的根域，然後按一下 **[確定]**。

    ![顯示 [變更網域] 對話方塊中 [確定] 按鈕的螢幕擷取畫面。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_44.gif)

4.  若要從 EA 群組移除所有成員：

    1.  按兩下 **Enterprise Admins** 群組，然後按一下 [ **成員** ] 索引標籤。

        ![顯示 Enterprise Admins 群組內 [成員] 索引標籤的螢幕擷取畫面。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_45.gif)

    2.  選取群組的成員，按一下 [ **移除**]，按一下 [ **是**]，然後按一下 **[確定]**。

5.  重複步驟2，直到移除 EA 群組的所有成員為止。

### <a name="step-by-step-instructions-to-secure-enterprise-admins-in-active-directory"></a>在 Active Directory 中保護企業系統管理員的逐步指示

1.  在 **伺服器管理員** 中，按一下 [ **工具**]，然後按一下 [ **群組原則管理**]。

2.  在主控台樹中，展開 [ <Forest> \Domains] \\ <Domain> ，然後 **群組原則物件** (其中 <Forest> 是樹系的名稱，而 <Domain> 是您要設定群組原則) 的網功能變數名稱稱。

    > [!NOTE]
    > 在包含多個網域的樹系中，您應該在需要保護 Enterprise Admins 群組的每個網域中建立類似的 GPO。

3.  在主控台樹中，以滑鼠右鍵按一下 **群組原則物件**]，然後按一下 [ **新增**]。

    ![顯示 [群組原則物件] 功能表中 [新增] 功能表選項的螢幕擷取畫面。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_46.gif)

4.  在 [ **新增 GPO** ] 對話方塊中，輸入 <GPO Name> ，然後按一下 **[確定]** (其中 <GPO Name> 是此 GPO) 的名稱。

    ![顯示要在哪裡輸入 GPO 名稱並選取來源入門 GPO 的螢幕擷取畫面。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_47.gif)

5.  在詳細資料窗格中，以滑鼠右鍵按一下 <GPO Name> ，然後按一下 [ **編輯**]。

6.  流覽至 [ **電腦設定 \windows 設定] \ 使用者設置 \ 本機原則**，然後按一下 [ **使用者權限指派**]。

    ![顯示如何選取使用者權限指派的螢幕擷取畫面。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_48.gif)

7.  設定使用者權限，以防止 Enterprise Admins 群組的成員透過網路存取成員伺服器和工作站，方法是執行下列動作：

    1.  按兩下 **[拒絕從網路存取這部電腦** ]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

    3.  輸入 **Enterprise Admins**，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

        ![示範如何確認您已設定使用者權限，以防止 Enterprise Admins 群組的成員透過網路存取成員伺服器和工作站的螢幕擷取畫面。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_49.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

8.  設定使用者權限，以防止 Enterprise Admins 群組的成員以批次工作登入，方法是執行下列動作：

    1.  按兩下 [ **拒絕以批次工作登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

        > [!NOTE]
        > 在包含多個網域的樹系中，按一下 [ **位置** ]，然後選取樹系的根域。

    3.  輸入 **Enterprise Admins**，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

        ![顯示如何確認您已設定使用者權限以防止 Enterprise Admins 群組的成員以批次工作登入的螢幕擷取畫面。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_50.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

9. 設定使用者權限，以防止 EA 群組的成員以服務的形式登入，方法是執行下列動作：

    1.  按兩下 [ **拒絕以服務方式登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

        > [!NOTE]
        > 在包含多個網域的樹系中，按一下 [ **位置** ]，然後選取樹系的根域。

    3.  輸入 **Enterprise Admins**，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

        ![顯示如何確認您已設定使用者權限以防止 EA 群組成員以服務方式登入的螢幕擷取畫面。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_51.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

10. 設定使用者權限，以防止 Enterprise Admins 群組的成員在本機登入成員伺服器和工作站，方法是執行下列動作：

    1.  按兩下 [ **拒絕本機登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

        > [!NOTE]
        > 在包含多個網域的樹系中，按一下 [ **位置** ]，然後選取樹系的根域。

    3.  輸入 **Enterprise Admins**，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

        ![顯示如何確認您已設定使用者權限，以防止 Enterprise Admins 群組成員在本機登入成員伺服器和工作站的螢幕擷取畫面。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_52.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

11. 設定使用者權限，以防止 Enterprise Admins 群組的成員透過遠端桌面服務執行下列動作，以防止企業系統管理員群組的成員存取成員伺服器和工作站：

    1.  按兩下 [ **拒絕透過遠端桌面服務登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

        > [!NOTE]
        > 在包含多個網域的樹系中，按一下 [ **位置** ]，然後選取樹系的根域。

    3.  輸入 **Enterprise Admins**，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

        ![示範如何確認您已設定使用者權限，以防止 Enterprise Admins 群組的成員透過遠端桌面服務存取成員伺服器和工作站的螢幕擷取畫面。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_53.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

12. 若要結束 **群組原則管理編輯器**，**請按一下 [** 檔案]，然後按一下 [結束 **]。**

13. 在 **群組原則管理**] 中，執行下列動作將 GPO 連結到成員伺服器和工作站 ou：

    1.  流覽至 <Forest> \Domains \\ <Domain> (其中 <Forest> 是樹系的名稱，而 <Domain> 是您要設定群組原則) 之網域的名稱。

    2.  以滑鼠右鍵按一下 GPO 將套用的 OU，然後按一下 [ **連結現有的 gpo**]。

        ![醒目顯示 [連結現有的 GPO] 功能表選項的螢幕擷取畫面。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_54.gif)

    3.  選取您剛才建立的 GPO，然後按一下 **[確定]**。

        ![顯示您剛才建立之 GPO 的螢幕擷取畫面。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_55.gif)

    4.  建立包含工作站之所有其他 Ou 的連結。

    5.  建立包含成員伺服器的所有其他 Ou 的連結。

    6.  在包含多個網域的樹系中，您應該在需要保護 Enterprise Admins 群組的每個網域中建立類似的 GPO。

> [!IMPORTANT]
> 如果使用跳躍伺服器來管理網域控制站和 Active Directory，請確定跳躍伺服器位於未連結此 Gpo 的 OU 中。

### <a name="verification-steps"></a>驗證步驟

#### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>確認「拒絕從網路存取這部電腦」 GPO 設定
從任何不受 GPO 影響的成員伺服器或工作站 (例如「跳躍伺服器」 ) ，嘗試透過受 GPO 變更影響的網路存取成員伺服器或工作站。 若要確認 GPO 設定，請執行下列步驟來嘗試使用 **NET USE** 命令對應系統磁片磁碟機：

1.  使用屬於 EA 群組成員的帳戶在本機登入。

2.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

3.  在 [ **搜尋** ] 方塊中，輸入 **命令提示** 字元，以滑鼠右鍵按一下 [ **命令提示** 字元]，然後按一下 [以 **系統管理員身分執行** ] 來開啟提升許可權的命令提示字元。

4.  當系統提示您核准提高許可權時，請按一下 **[是]**。

    ![顯示您核准提升許可權之對話方塊的螢幕擷取畫面。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_56.gif)

5.  在 [**命令提示** 字元] 視窗中，輸入 **net use \\ \\ \<Server Name\> \c $**，其中 \<Server Name\> 是您嘗試透過網路存取的成員伺服器或工作站的名稱。

6.  下列螢幕擷取畫面顯示應出現的錯誤訊息。

    ![顯示應該顯示之錯誤訊息的螢幕擷取畫面。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_57.gif)

#### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認「拒絕以批次工作登入」 GPO 設定

從受 GPO 變更影響的任何成員伺服器或工作站，在本機登入。

##### <a name="create-a-batch-file"></a>建立批次檔

1.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

2.  在 [ **搜尋** ] 方塊中，輸入 [ **記事本**]，然後按一下 [ **記事本**]。

3.  在 [ **記事本**] 中，輸入 **dir c：**。

4.  按一下 [檔案]，**然後按一下 [****另存** 新檔]。

5.  在 [**檔案名**] 方塊中，輸入 **<Filename> .bat** (其中 <Filename> 是) 的新批次檔的名稱。

##### <a name="schedule-a-task"></a>排程工作

1.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

2.  在 [ **搜尋** ] 方塊中 **，輸入 [** 工作排程器]，然後按一下 [ **工作排程器**]。

    > [!NOTE]
    > 在執行 Windows 8 的電腦上，于 [ **搜尋** ] 方塊中輸入 **排程** 工作，然後按一下 [ **排程** 工作]。

3.  按一下 [ **動作**]，然後按一下 [ **建立** 工作]。

4.  在 [ **建立** 工作] 對話方塊中，輸入 **<Task Name>** (其中 <Task Name> 是新工作) 的名稱。

5.  按一下 [ **動作** ] 索引標籤，然後按一下 [ **新增**]。

6.  在 [ **動作** ] 欄位中，選取 [ **啟動程式**]。

7.  在 [ **程式/腳本**] 底下，按一下 **[流覽]**，找出並選取 [ **建立批次檔** ] 區段中建立的批次檔，然後按一下 [ **開啟**]。

8.  按一下 [確定]。

9. 按一下 [General] \(一般\) 索引標籤。

10. 在 [ **安全性選項** ] 欄位中，按一下 [ **變更使用者或群組**]。

11. 輸入屬於 EAs 群組成員的帳戶名稱，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

12. 選取 [執行]， **不論使用者是否登入** ，然後選取 [ **不要儲存密碼**]。 此工作將只能存取本機電腦資源。

13. 按一下 [確定]。

14. 應該會出現一個對話方塊，要求使用者帳號憑證來執行工作。

15. 輸入認證之後，請按一下 **[確定]**。

16. 應該會出現類似下列的對話方塊。

    ![顯示 [工作排程器] 對話方塊的螢幕擷取畫面。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_58.gif)

#### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認「拒絕以服務方式登入」 GPO 設定

1.  從受 GPO 變更影響的任何成員伺服器或工作站，在本機登入。

2.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

3.  在 [ **搜尋** ] 方塊中，輸入 **服務**，然後按一下 [ **服務**]。

4.  找出並按兩下 [ **列印多工緩衝處理器**]。

5.  单击 **“登录”** 选项卡。

6.  在 [ **登入為**] 下，選取 [ **這個帳戶**]。

7.  按一下 **[流覽]**，輸入屬於 EAs 群組成員的帳戶名稱，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

8.  在 [ **密碼：** ] 和 [ **確認密碼**] 下，輸入選取的帳戶密碼，然後按一下 **[確定]**。

9. 再按三次 **[確定]** 。

10. 以滑鼠右鍵按一下 [ **列印多工緩衝處理器** ] 服務，然後選取 [ **重新開機**]。

11. 當服務重新開機時，應該會出現類似下列的對話方塊。

    ![顯示訊息的螢幕擷取畫面，指出 Windows 無法啟動列印多工緩衝處理器伺服器。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_59.gif)

#### <a name="revert-changes-to-the-printer-spooler-service"></a>還原印表機多工緩衝處理器服務的變更

1.  從受 GPO 變更影響的任何成員伺服器或工作站，在本機登入。

2.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

3.  在 [ **搜尋** ] 方塊中，輸入 **服務**，然後按一下 [ **服務**]。

4.  找出並按兩下 [ **列印多工緩衝處理器**]。

5.  单击 **“登录”** 选项卡。

6.  在 [ **登入為**] 下，選取 [ **本機系統** ] 帳戶，然後按一下 **[確定]**。

#### <a name="verify-deny-log-on-locally-gpo-settings"></a>確認「拒絕本機登入」 GPO 設定

1.  從受 GPO 變更影響的任何成員伺服器或工作站，嘗試使用屬於 EA 群組成員的帳戶在本機登入。 應該會出現類似下列的對話方塊。

    ![顯示訊息的螢幕擷取畫面，指出您所使用的登入方法不允許。](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_60.gif)

#### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>確認「拒絕登入遠端桌面服務」 GPO 設定

1.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

2.  在 [ **搜尋** ] 方塊中，輸入 **remote desktop connection**，然後按一下 [ **遠端桌面連線**]。

3.  在 [ **電腦** ] 欄位中，輸入您要連線的電腦名稱稱，然後按一下 [連線 **]**。  (您也可以輸入 IP 位址，而不是電腦名稱稱。 ) 

4.  出現提示時，請提供屬於 EA 群組成員之帳戶的認證。

5.  應該會出現類似下列的對話方塊。

    ![安全的企業系統管理員群組](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_61.gif)

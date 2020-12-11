---
description: 深入瞭解：附錄 F：保護 Active Directory 中的 Domain Admins 群組
ms.assetid: 017b88a6-f29b-4787-99b6-b5c8eaf8c3df
title: 附錄 F-保護 Active Directory 中的 Domain Admins 群組
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 591cab5cdba949b7f1828a6719904a2beae071e5
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97041636"
---
# <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>附錄 F︰保護 Active Directory 中的 Domain Admins 群組

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>附錄 F︰保護 Active Directory 中的 Domain Admins 群組
就像 Enterprise Admins (EA) 群組一樣，只有在建立或嚴重損壞修復案例中，才需要 Domain Admins (DA) 群組中的成員資格。 因為網域的內建系統管理員帳戶已受到保護，所以在 DA 群組中應該沒有任何日常使用者帳戶，因為網域的內建系統管理員帳戶受到保護，如 [附錄 D：保護 Built-In 系統管理員帳戶 Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)中所述。

根據預設，網域系統管理員會在其個別網域中的所有成員伺服器和工作站上，擁有本機系統管理員群組的成員。 此預設的嵌套不應修改為可支援性和嚴重損壞修復的目的。 如果網域系統管理員已從成員伺服器上的本機系統管理員群組中移除，則應該將群組新增到每個成員伺服器上的 [Administrators] 群組和網域中的 [工作站]。 每個網域的 Domain Admins 群組都應受到保護，如後續的逐步指示所述。

針對樹系中每個網域的 Domain Admins 群組：

1.  移除群組中的所有成員，但網域的內建 Administrator 帳戶可能除外，但前提是已依照 [附錄 D：保護 Built-In 系統管理員帳戶 Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)中所述的方式進行保護。

2.  在連結到包含每個網域中成員伺服器和工作站之 Ou 的 Gpo 中，應將該 DA 群組新增至 **電腦設定 \windows 設定設定 \** 使用者 \ 使用者 \ 使用者 \ 使用者權限指派中的下列使用者權限：

    -   拒絕從網路存取這部電腦

    -   拒絕以批次工作登入

    -   拒絕以服務方式登入

    -   拒絕本機登入

    -   拒絕透過遠端桌面服務使用者權限登入

3.  如果對 Domain Admins 群組的屬性或成員資格進行任何修改，則應該將審核設定為傳送警示。

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-domain-admins-group"></a>從 Domain Admins 群組移除所有成員的逐步指示

1.  在 **伺服器管理員** 中，按一下 [ **工具**]，然後按一下 [ **Active Directory 消費者和電腦**]。

2.  若要移除 DA 群組中的所有成員，請執行下列步驟：

    1.  按兩下 **Domain Admins** 群組，然後按一下 [ **成員** ] 索引標籤。

        ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_62.gif)

    2.  選取群組的成員，按一下 [ **移除**]，按一下 [ **是**]，然後按一下 **[確定]**。

3.  重複步驟2，直到已移除 DA 群組的所有成員為止。

#### <a name="step-by-step-instructions-to-secure-domain-admins-in-active-directory"></a>在 Active Directory 中保護 Domain Admins 的逐步指示

1.  在 **伺服器管理員** 中，按一下 [ **工具**]，然後按一下 [ **群組原則管理**]。

2.  在主控台樹中，展開 [ \<Forest\> \\ 網域] \\ \<Domain\> ，然後 **群組原則物件**] (其中 \<Forest\> 是樹系的名稱，而則 \<Domain\> 是您要設定群組原則) 的網功能變數名稱稱。

3.  在主控台樹中，以滑鼠右鍵按一下 **群組原則物件**]，然後按一下 [ **新增**]。

    ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_63.gif)

4.  在 [ **新增 GPO** ] 對話方塊中，輸入 \<GPO Name\> ，然後按一下 **[確定]** (其中 \<GPO Name\> 是此 GPO) 的名稱。

    ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_64.gif)

5.  在詳細資料窗格中，以滑鼠右鍵按一下 \<GPO Name\> ，然後按一下 [ **編輯**]。

6.  流覽至 [ **電腦設定 \windows 設定] \ 使用者設置 \ 本機原則**，然後按一下 [ **使用者權限指派**]。

    ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_65.gif)

7.  設定使用者權限，以防止 Domain Admins 群組的成員透過網路存取成員伺服器和工作站，方法是執行下列動作：

    1.  按兩下 **[拒絕從網路存取這部電腦** ]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

    3.  輸入 **Domain Admins**，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

        ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_66.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

8.  設定使用者權限，以防止 DA 群組的成員以批次工作登入，方法是執行下列動作：

    1.  按兩下 [ **拒絕以批次工作登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

    3.  輸入 **Domain Admins**，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

        ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_67.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

9. 設定使用者權限，以防止 DA 群組的成員以服務的形式登入，方法是執行下列動作：

    1.  按兩下 [ **拒絕以服務方式登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

    3.  輸入 **Domain Admins**，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

        ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_68.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

10. 設定使用者權限，以防止 Domain Admins 群組的成員在本機登入成員伺服器和工作站，方法是執行下列動作：

    1.  按兩下 [ **拒絕本機登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

    3.  輸入 **Domain Admins**，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

        ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_69.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

11. 藉由執行下列動作，設定使用者權限以防止 Domain Admins 群組的成員透過遠端桌面服務存取成員伺服器和工作站：

    1.  按兩下 [ **拒絕透過遠端桌面服務登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

    3.  輸入 **Domain Admins**，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

        ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_70.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

12. 若要結束 **群組原則管理編輯器**，**請按一下 [** 檔案]，然後按一下 [結束 **]。**

13. 在群組原則管理] 中，執行下列動作將 GPO 連結到成員伺服器和工作站 Ou：

    1.  流覽至 \<Forest\> \Domains \\ \<Domain\> (其中 \<Forest\> 是樹系的名稱，而 \<Domain\> 是您要設定群組原則) 之網域的名稱。

    2.  以滑鼠右鍵按一下 GPO 將套用的 OU，然後按一下 [ **連結現有的 gpo**]。

        ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_71.gif)

    3.  選取您剛才建立的 GPO，然後按一下 **[確定]**。

        ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_72.gif)

    4.  建立包含工作站之所有其他 Ou 的連結。

    5.  建立包含成員伺服器的所有其他 Ou 的連結。

        > [!IMPORTANT]
        > 如果使用跳躍伺服器來管理網域控制站和 Active Directory，請確定跳躍伺服器位於未連結此 Gpo 的 OU 中。

#### <a name="verification-steps"></a>驗證步驟

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>確認「拒絕從網路存取這部電腦」 GPO 設定
從任何不受 GPO 影響的成員伺服器或工作站 (例如「跳躍伺服器」 ) ，嘗試透過受 GPO 變更影響的網路存取成員伺服器或工作站。 若要確認 GPO 設定，請使用 **NET USE** 命令嘗試對應系統磁片磁碟機。

1.  使用 Domain Admins 群組成員的帳戶在本機登入。

2.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

3.  在 [ **搜尋** ] 方塊中，輸入 **命令提示** 字元，以滑鼠右鍵按一下 [ **命令提示** 字元]，然後按一下 [以 **系統管理員身分執行** ] 來開啟提升許可權的命令提示字元。

4.  當系統提示您核准提高許可權時，請按一下 **[是]**。

    ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_73.gif)

5.  在 [**命令提示** 字元] 視窗中，輸入 **net use \\ \\ \<Server Name\> \c $**，其中 \<Server Name\> 是您嘗試透過網路存取的成員伺服器或工作站的名稱。

6.  下列螢幕擷取畫面顯示應出現的錯誤訊息。

    ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_74.gif)

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認「拒絕以批次工作登入」 GPO 設定

從受 GPO 變更影響的任何成員伺服器或工作站，在本機登入。

###### <a name="create-a-batch-file"></a>建立批次檔

1.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

2.  在 [ **搜尋** ] 方塊中，輸入 [ **記事本**]，然後按一下 [ **記事本**]。

3.  在 [ **記事本**] 中，輸入 **dir c：**。

4.  按一下 [檔案]，**然後按一下 [****另存** 新檔]。

5.  在 [**檔案名**] 欄位中，輸入 **\<Filename\> .bat** (其中 \<Filename\> 是) 的新批次檔的名稱。

###### <a name="schedule-a-task"></a>排程工作

1.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

2.  在 [ **搜尋** ] 方塊中 **，輸入 [** 工作排程器]，然後按一下 [ **工作排程器**]。

    > [!NOTE]
    > 在執行 Windows 8 的電腦上，于 [ **搜尋** ] 方塊中輸入 **排程** 工作，然後按一下 [ **排程** 工作]。

3.  在 [ **工作排程器** ] 功能表列中，按一下 [ **動作**]，然後按一下 [ **建立** 工作]。

4.  在 [ **建立** 工作] 對話方塊中，輸入 **\<Task Name\>** (其中 \<Task Name\> 是新工作) 的名稱。

5.  按一下 [ **動作** ] 索引標籤，然後按一下 [ **新增**]。

6.  在 [ **動作** ] 欄位中，選取 [ **啟動程式**]。

7.  在 [ **程式/腳本**] 底下，按一下 **[流覽]**，找出並選取 [ **建立批次檔** ] 區段中建立的批次檔，然後按一下 [ **開啟**]。

8.  按一下 [確定]。

9. 按一下 [General] \(一般\) 索引標籤。

10. 在 [ **安全性** 選項] 底下，按一下 [ **變更使用者或群組**]。

11. 輸入屬於 Domain Admins 群組成員的帳戶名稱，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

12. 選取 [執行]， **不論使用者是否登入** ，然後選取 [ **不要儲存密碼**]。 此工作將只能存取本機電腦資源。

13. 按一下 [確定]。

14. 應該會出現一個對話方塊，要求使用者帳號憑證來執行工作。

15. 輸入認證之後，請按一下 **[確定]**。

16. 應該會出現類似下列的對話方塊。

    ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_75.gif)

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認「拒絕以服務方式登入」 GPO 設定

1.  從受 GPO 變更影響的任何成員伺服器或工作站，在本機登入。

2.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

3.  在 [ **搜尋** ] 方塊中，輸入 **服務**，然後按一下 [ **服務**]。

4.  找出並按兩下 [ **列印多工緩衝處理器**]。

5.  单击 **“登录”** 选项卡。

6.  在 [ **登入為**] 下，選取 [ **此帳戶** ] 選項。

7.  按一下 **[流覽]**，輸入屬於 Domain Admins 群組成員的帳戶名稱，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

8.  在 [ **密碼** ] 和 [ **確認密碼**] 下，輸入選取的帳戶密碼，然後按一下 **[確定]**。

9. 再按三次 **[確定]** 。

10. 以滑鼠右鍵按一下 [ **列印多工緩衝處理器** ]，然後按一下 [ **重新開機**]

11. 當服務重新開機時，應該會出現類似下列的對話方塊。

    ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_76.gif)

##### <a name="revert-changes-to-the-printer-spooler-service"></a>還原印表機多工緩衝處理器服務的變更

1.  從受 GPO 變更影響的任何成員伺服器或工作站，在本機登入。

2.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

3.  在 [ **搜尋** ] 方塊中，輸入 **服務**，然後按一下 [ **服務**]。

4.  找出並按兩下 [ **列印多工緩衝處理器**]。

5.  单击 **“登录”** 选项卡。

6.  在 [ **登入為**] 下，選取 [ **本機系統** ] 帳戶，然後按一下 **[確定]**。

##### <a name="verify-deny-log-on-locally-gpo-settings"></a>確認「拒絕本機登入」 GPO 設定

1.  從受 GPO 變更影響的任何成員伺服器或工作站，嘗試使用 Domain Admins 群組成員的帳戶在本機登入。 應該會出現類似下列的對話方塊。

    ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_77.gif)

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>確認「拒絕登入遠端桌面服務」 GPO 設定
1.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

2.  在 [ **搜尋** ] 方塊中，輸入 **remote desktop connection**，然後按一下 [ **遠端桌面連線**]。

3.  在 [ **電腦** ] 欄位中，輸入您要連線的電腦名稱稱，然後按一下 [連線 **]**。  (您也可以輸入 IP 位址，而不是電腦名稱稱。 ) 

4.  出現提示時，請提供屬於 Domain Admins 群組成員之帳戶的認證。

5.  應該會出現類似下列的對話方塊。

    ![安全網域系統管理員群組](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_78.gif)

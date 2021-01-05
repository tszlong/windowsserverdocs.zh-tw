---
description: 深入瞭解：附錄 G：保護 Active Directory 中的系統管理員群組
ms.assetid: 4baefbd3-038f-44c0-85ba-f24e9722b757
title: 附錄 G-保護 Active Directory 中的系統管理員群組
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 93ddd6cf87b3736895b15ff185e43dc0e98da78c
ms.sourcegitcommit: 3247e193d9fe1b57543fff215460a6d9db52f58b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/30/2020
ms.locfileid: "97815027"
---
# <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>附錄 G︰保護 Active Directory 中的 Administrators 群組

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>附錄 G︰保護 Active Directory 中的 Administrators 群組
就像 Enterprise Admins (EA) 和 Domain Admins (DA) 群組一樣，內建系統管理員 (BA) 群組中的成員資格，只有在建立或嚴重損壞修復案例中才需要。 除了網域的內建系統管理員帳戶（如 [附錄 D：保護 Active Directory 中的 Built-In 系統管理員](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)帳戶）中所述，系統管理員群組中應該不會有每天的使用者帳戶，但是網域內建的系統管理員帳戶除外。

依預設，系統管理員會在其各自的網域中擁有大多數 AD DS 物件的擁有者。 需要擁有權或取得物件擁有權之能力的組建或嚴重損壞修復案例中，可能需要此群組的成員資格。 此外，DAs 和 EAs 透過其 Administrators 群組中的預設成員資格，繼承了許多權利和許可權。 Active Directory 中的特殊許可權群組的預設群組嵌套不應該修改，而且每個網域的系統管理員群組都應受到保護，如下列逐步指示所述。

針對樹系中每個網域的 Administrators 群組：

1.  移除 Administrators 群組中的所有成員，但網域的內建 Administrator 帳戶可能除外，但前提是已依照 [附錄 D：保護 Built-In 系統管理員帳戶 Active Directory 中](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)所述的方式進行保護。

2.  在連結到包含成員伺服器和每個網域中工作站之 Ou 的 Gpo 中，您應該將 BA 群組新增至電腦設定 \windows 設定設為 [ **本機原則 \ 使用者權限指派**] 中的下列使用者權限：

    -   拒絕從網路存取這部電腦

    -   拒絕以批次工作登入

    -   拒絕以服務方式登入

3.  在樹系中每個網域的網域控制站 OU 上，系統管理員群組應獲得下列使用者權限：

    -   從網路存取這台電腦

    -   允許本機登入

    -   允許透過遠端桌面服務登入

4.  如果對系統管理員群組的屬性或成員資格進行任何修改，則應該將審核設定為傳送警示。

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-administrators-group"></a>從 Administrators 群組移除所有成員的逐步指示

1.  在 **伺服器管理員** 中，按一下 [ **工具**]，然後按一下 [ **Active Directory 消費者和電腦**]。

2.  若要移除 Administrators 群組中的所有成員，請執行下列步驟：

    1.  按兩下 [系統 **管理員** ] 群組，然後按一下 [ **成員** ] 索引標籤。

        ![顯示 [成員] 索引標籤的螢幕擷取畫面，用來移除 Administrators 群組中的所有成員。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_79.gif)

    2.  選取群組的成員，按一下 [ **移除**]，按一下 [ **是**]，然後按一下 **[確定]**。

3.  重複步驟2，直到系統管理員群組的所有成員都已移除為止。

#### <a name="step-by-step-instructions-to-secure-administrators-groups-in-active-directory"></a>在 Active Directory 中保護系統管理員群組的逐步指示

1.  在 **伺服器管理員** 中，按一下 [ **工具**]，然後按一下 [ **群組原則管理**]。

2.  在主控台樹中，展開 [ &lt; 樹系 &gt; \Domains 網域] \\ &lt; &gt; ，然後 **群組原則 [物件** (其中樹系 &lt; &gt; 是樹系的名稱，而 &lt; 網域 &gt; 是您要設定群組原則) 的網功能變數名稱稱。

3.  在主控台樹中，以滑鼠右鍵按一下 **群組原則物件**]，然後按一下 [ **新增**]。

    ![螢幕擷取畫面：顯示要在哪裡選取 [新增]，以便您可以在 Active Directory 中保護系統管理員群組。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_80.gif)

4.  在 [ **新增 GPO** ] 對話方塊中，輸入 <GPO Name> ，然後按一下 **[確定]** (其中 *gpo 名稱* 是此 gpo) 的名稱。

    ![螢幕擷取畫面，顯示在 [新增 GPO] 對話方塊中，將 G P O 命名的位置，讓您可以保護系統管理員群組。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_81.gif)

5.  在詳細資料窗格中，以滑鼠右鍵按一下 **<GPO Name>** ，然後按一下 [ **編輯**]。

6.  流覽至 [ **電腦設定 \windows 設定] \ 使用者設置 \ 本機原則**，然後按一下 [ **使用者權限指派**]。

    ![顯示流覽位置的螢幕擷取畫面，讓您可以選取使用者權限管理選項來保護系統管理員群組。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_82.gif)

7.  設定使用者權限，以防止系統管理員群組的成員透過網路存取成員伺服器和工作站，方法是執行下列動作：

    1.  按兩下 **[拒絕從網路存取這部電腦** ]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

    3.  輸入 [系統 **管理員**]，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

        ![螢幕擷取畫面，顯示如何確認您已設定使用者權限，以防止 Administrators 群組的成員透過網路存取成員伺服器和工作站。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_83.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

8.  設定使用者權限，以防止系統管理員群組的成員以批次工作登入，方法是執行下列動作：

    1.  按兩下 [ **拒絕以批次工作登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

    3.  輸入 [系統 **管理員**]，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

        ![顯示如何確認您已設定使用者權限以防止系統管理員群組的成員以批次工作登入的螢幕擷取畫面。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_84.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

9. 設定使用者權限，以防止 Administrators 群組的成員以服務的身分登入，方法是執行下列動作：

    1.  按兩下 [ **拒絕以服務方式登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

    3.  輸入 [系統 **管理員**]，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

        ![螢幕擷取畫面，顯示如何確認您已設定使用者權限，以防止系統管理員群組的成員以服務的身分登入。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_85.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

10. 若要結束 **群組原則管理編輯器**，**請按一下 [** 檔案]，然後按一下 [結束 **]。**

11. 在 **群組原則管理**] 中，執行下列動作將 GPO 連結到成員伺服器和工作站 ou：

    1.  流覽至樹 &lt; 系 &gt;> \domains \\ &lt; 網域 &gt; (其中 &lt; 樹系 &gt; 是樹系的名稱，而 &lt; 網域 &gt; 是您要設定群組原則) 的網功能變數名稱稱。

    2.  以滑鼠右鍵按一下 GPO 將套用的 OU，然後按一下 [ **連結現有的 gpo**]。

        ![當您在 OU 上按一下滑鼠右鍵時，顯示 [連結到現有的 G P O] 功能表選項的螢幕擷取畫面。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_86.gif)

    3.  選取您剛才建立的 GPO，然後按一下 **[確定]**。

        ![顯示您剛才建立之 GPO 的螢幕擷取畫面。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_87.gif)

    4.  建立包含工作站之所有其他 Ou 的連結。

    5.  建立包含成員伺服器的所有其他 Ou 的連結。

        > [!IMPORTANT]
        > 如果使用跳躍伺服器來管理網域控制站和 Active Directory，請確定跳躍伺服器位於未連結此 Gpo 的 OU 中。

        > [!NOTE]
        > 當您在 Gpo 的 Administrators 群組上執行限制時，除了網域的 Administrators 群組之外，Windows 也會將設定套用至電腦本機系統管理員群組的成員。 因此，當您在 Administrators 群組中實施限制時，應謹慎使用。 雖然在可以執行的任何地方都不允許系統管理員群組成員的網路、批次和服務登入，但是請不要透過遠端桌面服務限制本機登入或登入。 封鎖這些登入類型可能會封鎖本機系統管理員群組成員的電腦合法管理。
        >
        > 下列螢幕擷取畫面顯示的設定會封鎖內建本機和網域系統管理員帳戶的誤用，以及誤用內建的本機或網域系統管理員群組。 請注意，[ **拒絕登入遠端桌面服務** ] 使用者權限不包含 [系統管理員] 群組，因為在這項設定中包含此群組，也會針對屬於本機電腦的 Administrators 群組成員的帳戶封鎖這些登入。 如果電腦上的服務設定為在本節所述的任何特殊許可權群組內容中執行，則執行這些設定可能會導致服務和應用程式失敗。 因此，與本節中的所有建議相同，您應該在您的環境中徹底測試適用性的設定。

        ![顯示設定的螢幕擷取畫面，會封鎖內建本機和網域系統管理員帳戶的誤用。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_88.gif)

#### <a name="step-by-step-instructions-to-grant-user-rights-to-the-administrators-group"></a>將使用者權限授與系統管理員群組的逐步指示

1.  在 **伺服器管理員** 中，按一下 [ **工具**]，然後按一下 [ **群組原則管理**]。

2.  在主控台樹中，展開 [ <Forest> \Domains] \\ <Domain> ，然後 **群組原則物件** (其中 <Forest> 是樹系的名稱，而 <Domain> 是您要設定群組原則) 的網功能變數名稱稱。

3.  在主控台樹中，以滑鼠右鍵按一下 **群組原則物件**]，然後按一下 [ **新增**]。

    ![顯示當您在群組原則物件上按一下滑鼠右鍵時所顯示功能表的螢幕擷取畫面。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_89.gif)

4.  在 [ **新增 GPO** ] 對話方塊中，輸入 <GPO Name> ，然後按一下 **[確定]** (其中 <GPO Name> 是此 GPO) 的名稱。

    ![顯示要在哪裡命名 G P O 的螢幕擷取畫面，讓您可以保護系統管理員群組。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_90.gif)

5.  在詳細資料窗格中，以滑鼠右鍵按一下 **<GPO Name>** ，然後按一下 [ **編輯**]。

6.  流覽至 [ **電腦設定 \windows 設定] \ 使用者設置 \ 本機原則**，然後按一下 [ **使用者權限指派**]。

    ![顯示流覽位置的螢幕擷取畫面，讓您可以選取使用者權限管理員來保護系統管理員群組。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_91.gif)

7.  藉由執行下列動作來設定使用者權限，讓 Administrators 群組的成員可以透過網路存取網域控制站：

    1.  按兩下 **[從網路存取這部電腦** ]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

    3.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

        ![示範如何確認您已將使用者權限設定為允許 Administrators 群組成員透過網路存取網域控制站的螢幕擷取畫面。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_92.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

8.  藉由執行下列動作，設定使用者權限以允許 Administrators 群組的成員在本機登入：

    1.  按兩下 [ **允許本機登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

    3.  輸入 [系統 **管理員**]，按一下 [檢查 **名稱**]，然後按一下 **[確定]**。

        ![顯示如何確認您已設定使用者權限以允許 Administrators 群組成員在本機登入的螢幕擷取畫面。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_93.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

9. 藉由執行下列動作，設定使用者權限以允許 Administrators 群組的成員登入遠端桌面服務：

    1.  按兩下 [ **允許透過遠端桌面服務登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

    3.  輸入 [系統 **管理員**]，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

        ![螢幕擷取畫面，顯示如何確認您已設定使用者權限，讓系統管理員群組的成員可以透過遠端桌面服務登入。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_94.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

10. 若要結束 **群組原則管理編輯器**，**請按一下 [** 檔案]，然後按一下 [結束 **]。**

11. 在 **群組原則管理**] 中，執行下列動作將 GPO 連結至網域控制站 OU：

    1.  流覽至 <Forest> \Domains \\ <Domain> (其中 <Forest> 是樹系的名稱，而 <Domain> 是您要設定群組原則) 之網域的名稱。

    2.  在 [網域控制站] OU 上按一下滑鼠右鍵，然後按一下 [ **連結到現有的 GPO**]。

        ![當您嘗試將 G P O 連結至網域控制站 OU 時，顯示 [連結現有的 GPO] 功能表選項的螢幕擷取畫面。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_95.gif)

    3.  選取您剛才建立的 GPO，然後按一下 **[確定]**。

        ![顯示在將 G P O 連結至成員工作站和伺服器的時候，顯示您剛才建立之 GPO 的螢幕擷取畫面。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_96.gif)

#### <a name="verification-steps"></a>驗證步驟

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>確認「拒絕從網路存取這部電腦」 GPO 設定
從任何不受 GPO 影響的成員伺服器或工作站 (例如「跳躍伺服器」 ) ，嘗試透過受 GPO 變更影響的網路存取成員伺服器或工作站。 若要確認 GPO 設定，請使用 **NET USE** 命令嘗試對應系統磁片磁碟機。

1.  使用屬於系統管理員群組成員的帳戶在本機登入。

2.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

3.  在 [ **搜尋** ] 方塊中，輸入 **命令提示** 字元，以滑鼠右鍵按一下 [ **命令提示** 字元]，然後按一下 [以 **系統管理員身分執行** ] 來開啟提升許可權的命令提示字元。

4.  當系統提示您核准提高許可權時，請按一下 **[是]**。

    ![醒目顯示 [使用者帳戶控制] 對話方塊的螢幕擷取畫面。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_97.gif)

5.  在 [**命令提示** 字元] 視窗中，輸入 **net use \\ \\ \<Server Name\> \c $**，其中 \<Server Name\> 是您嘗試透過網路存取的成員伺服器或工作站的名稱。

6.  下列螢幕擷取畫面顯示應出現的錯誤訊息。

    ![反白顯示登入失敗錯誤訊息的螢幕擷取畫面。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_98.gif)

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認「拒絕以批次工作登入」 GPO 設定
從受 GPO 變更影響的任何成員伺服器或工作站，在本機登入。

###### <a name="create-a-batch-file"></a>建立批次檔

1.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

2.  在 [ **搜尋** ] 方塊中，輸入 [ **記事本**]，然後按一下 [ **記事本**]。

3.  在 [ **記事本**] 中，輸入 **dir c：**。

4.  按一下 [檔案]，**然後按一下 [****另存** 新檔]。

5.  在 [**檔案名**] 欄位中，輸入 **<Filename> .bat** (其中 <Filename> 是) 的新批次檔的名稱。

###### <a name="schedule-a-task"></a>排程工作

1.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

2.  在 [ **搜尋** ] 方塊中 **，輸入 [** 工作排程器]，然後按一下 [ **工作排程器**]。

    > [!NOTE]
    > 在執行 Windows 8 的電腦上，于 [搜尋] 方塊中輸入排程工作，然後按一下 [排程工作]。

3.  按一下 [ **動作**]，然後按一下 [ **建立** 工作]。

4.  在 [ **建立** 工作] 對話方塊中，輸入 **<Task Name>** (其中 <Task Name> 是新工作) 的名稱。

5.  按一下 [ **動作** ] 索引標籤，然後按一下 [ **新增**]。

6.  在 [ **動作** ] 欄位中，選取 [ **啟動程式**]。

7.  在 [ **程式/腳本** ] 欄位中，按一下 **[流覽]**，找出並選取 [ **建立批次檔** ] 區段中建立的批次檔，然後按一下 [ **開啟**]。

8.  按一下 [確定]。

9. 按一下 [General] \(一般\) 索引標籤。

10. 在 [ **安全性選項** ] 欄位中，按一下 [ **變更使用者或群組**]。

11. 輸入屬於系統管理員群組成員的帳戶名稱，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

12. 選取 **[執行]，不論使用者是否登入** ，並 **不要儲存密碼**。 此工作將只能存取本機電腦資源。

13. 按一下 [確定]。

14. 應該會出現一個對話方塊，要求使用者帳號憑證來執行工作。

15. 輸入密碼之後，按一下 **[確定]**。

16. 應該會出現類似下列的對話方塊。

    ![醒目顯示 [工作排程器] 對話方塊的螢幕擷取畫面。](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_99.gif)

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認「拒絕以服務方式登入」 GPO 設定

1.  從受 GPO 變更影響的任何成員伺服器或工作站，在本機登入。

2.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

3.  在 [ **搜尋** ] 方塊中，輸入 **服務**，然後按一下 [ **服務**]。

4.  找出並按兩下 [ **列印多工緩衝處理器**]。

5.  单击 **“登录”** 选项卡。

6.  在 [ **登入為** ] 欄位中，選取 [ **這個帳戶**]。

7.  按一下 **[流覽]**，輸入屬於系統管理員群組成員的帳戶名稱，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

8.  在 [ **密碼** ] 和 [ **確認密碼** ] 欄位中，輸入選取的帳戶密碼，然後按一下 **[確定]**。

9. 再按三次 **[確定]** 。

10. 以滑鼠右鍵按一下 [ **列印多工緩衝處理器** ]，然後按一下 [ **重新開機**]

11. 當服務重新開機時，應該會出現類似下列的對話方塊。

    ![保護系統管理群組](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_100.png)

##### <a name="revert-changes-to-the-printer-spooler-service"></a>還原印表機多工緩衝處理器服務的變更

1.  從受 GPO 變更影響的任何成員伺服器或工作站，在本機登入。

2.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

3.  在 [ **搜尋** ] 方塊中，輸入 **服務**，然後按一下 [ **服務**]。

4.  找出並按兩下 [ **列印多工緩衝處理器**]。

5.  单击 **“登录”** 选项卡。

6.  在 [ **登入為** ] 欄位中，按一下 [ **本機系統** 帳戶]，然後按一下 **[確定]**。

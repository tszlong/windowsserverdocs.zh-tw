---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: 附錄 D-在 Active Directory 中保護內建的系統管理員帳戶
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: efd0ac56d3e9f0480ed59e50d42f7e99d416f22d
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941628"
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>附錄 D︰保護 Active Directory 中的內建的 Administrator 帳戶

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>附錄 D︰保護 Active Directory 中的內建的 Administrator 帳戶
在 Active Directory 的每個網域中，系統會在建立網域的過程中建立系統管理員帳戶。 此帳戶預設為網域中 Domain Admins 和 Administrators 群組的成員，如果網域是樹系根域，則帳戶也是 Enterprise Admins 群組的成員。

使用網域的系統管理員帳戶應該僅保留給初始組建活動，以及可能的嚴重損壞修復案例。 若要確保在沒有其他帳戶可用的情況下，可以使用系統管理員帳戶來生效修復，您不應該在樹系的任何網域中變更系統管理員帳戶的預設成員資格。 相反地，您應該在樹系中的每個網域中保護系統管理員帳戶，如下一節所述，並詳細說明後續的逐步指示。

> [!NOTE]
> 本指南可用來建議停用帳戶。 這已移除，因為樹系修復白皮書會使用預設的系統管理員帳戶。 原因是，這是唯一允許無通用類別目錄伺服器登入的帳戶。


#### <a name="controls-for-built-in-administrator-accounts"></a>內建系統管理員帳戶的控制項
針對樹系中每個網域內的內建系統管理員帳戶，您應該設定下列設定：

-   啟用 **帳戶為機密，而且無法** 在帳戶上委派旗標。

-   在帳戶上啟用 **互動式登入旗標所需的智慧卡** 。

-   設定 Gpo 以限制系統管理員帳戶在已加入網域的系統上的使用：

    -   在您建立並連結到每個網域中的工作站和成員伺服器 Ou 的一或多個 Gpo 中，將每個網域的系統管理員帳戶新增至電腦設定 \windows 設定設定 \ 使用者權限 \ 使用者 **權力指派**中的下列使用者權限：

        -   拒絕從網路存取這部電腦

        -   拒絕以批次工作登入

        -   拒絕以服務方式登入

        -   拒絕透過遠端桌面服務登入

> [!NOTE]
> 當您將帳戶加入此設定時，您必須指定是否要設定本機系統管理員帳戶或網域系統管理員帳戶。 例如，若要將 NWTRADERS 網域的系統管理員帳戶新增到這些拒絕許可權，您必須將帳戶輸入為 NWTRADERS\Administrator，或流覽至 NWTRADERS 網域的系統管理員帳戶。 如果您在群組原則物件編輯器的這些使用者權限設定中輸入「系統管理員」，您將會限制套用 GPO 之每部電腦上的本機系統管理員帳戶。
>
> 建議您以網域系統管理員帳戶的相同方式，限制成員伺服器和工作站上的本機系統管理員帳戶。 因此，您通常應該將樹系中每個網域的系統管理員帳戶，以及本機電腦的系統管理員帳戶新增至這些使用者權限設定。 下列螢幕擷取畫面顯示設定這些使用者權限以封鎖本機系統管理員帳戶的範例，以及網域的系統管理員帳戶，使其無法執行這些帳戶不需要的登入。


![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)

-   設定 Gpo 以限制網域控制站上的系統管理員帳戶
    -   在樹系中的每個網域中，您應該修改預設的網域控制站 GPO 或連結至網域控制站 OU 的原則，以將每個網域的系統管理員帳戶新增至電腦設定 \windows 設定設定 \ 使用者設定 \ 使用者權限 \ 使用者 **權力指派**中的下列使用者權限：
        -   拒絕從網路存取這部電腦

        -   拒絕以批次工作登入

        -   拒絕以服務方式登入

        -   拒絕透過遠端桌面服務登入

> [!NOTE]
> 這些設定可確保網域的內建系統管理員帳戶不能用來連線到網域控制站，雖然帳戶（如果啟用的話）可以在本機登入網域控制站。 因為此帳戶應該只在嚴重損壞修復的情況下啟用及使用，所以會預期至少有一個網域控制站的實體存取可供使用，或者可以使用具有遠端存取網域控制站許可權的其他帳戶。

-   設定系統管理員帳戶的審核

    當您保護每個網域的系統管理員帳戶並停用它時，您應該設定審核來監視帳戶的變更。 如果帳戶已啟用，則會重設其密碼，或對帳戶進行任何其他修改，警示應傳送給負責管理 Active Directory 的使用者或小組，以及組織中的事件回應小組。

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>在 Active Directory 中保護內建系統管理員帳戶的逐步指示

1.  在 **伺服器管理員**中，按一下 [ **工具**]，然後按一下 [ **Active Directory 消費者和電腦**]。

2.  若要防止利用委派在其他系統上使用帳號憑證的攻擊，請執行下列步驟：

    1.  在 **系統管理員** 帳戶上按一下滑鼠右鍵， **然後按一下 [** 內容]。

    2.  按一下 [帳戶]  索引標籤。

    3.  在 [ **帳戶選項**] 下，選取 [ **帳戶是機密的，無法委派** 旗標]，然後按一下 **[確定]**。

        ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)

3.  若要啟用帳戶上 **互動式登入旗標所需的智慧卡** ，請執行下列步驟：

    1.  在 **系統管理員** 帳戶上按一下滑鼠右鍵，然後選取 [ **屬性**]。

    2.  按一下 [帳戶]  索引標籤。

    3.  在 [ **帳戶** 選項] 下，選取 [ **互動式登入旗標需要智慧卡** ] 旗標，如下列螢幕擷取畫面所示，然後按一下 **[確定]**。

        ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif)

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>設定 Gpo 以限制網域層級的系統管理員帳戶

> [!WARNING]
> 此 GPO 永遠不應連結到網域層級，因為它可以讓內建的系統管理員帳戶無法使用，即使在嚴重損壞修復案例中也一樣。

1.  在 **伺服器管理員**中，按一下 [ **工具**]，然後按一下 [ **群組原則管理**]。

2.  在主控台樹中，展開 [ <Forest> \Domains] \\ <Domain> ，然後**群組原則物件** (其中 <Forest> 是樹系的名稱，而 <Domain> 是您要在其中建立群組原則) 之網域的名稱。

3.  在主控台樹中，以滑鼠右鍵按一下 **群組原則物件**]，然後按一下 [ **新增**]。

    ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)

4.  在 [ **新增 GPO** ] 對話方塊中，輸入 <GPO Name> ，然後按一下 **[確定]** (其中 <GPO Name> 是此 GPO 的名稱) 如下列螢幕擷取畫面所示。

    ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)

5.  在詳細資料窗格中，以滑鼠右鍵按一下 <GPO Name> ，然後按一下 [ **編輯**]。

6.  流覽至 [ **電腦設定 \windows 設定] \ 使用者設置 \ 本機原則**，然後按一下 [ **使用者權限指派**]。

    ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)

7.  設定使用者權限，以防止系統管理員帳戶透過網路存取成員伺服器和工作站，請執行下列動作：

    1.  按兩下 **[拒絕從網路存取這部電腦** ]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

    3.  輸入 [ **系統管理員**]，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。 確認帳戶以 \Username 格式顯示， <DomainName> 如下列螢幕擷取畫面所示。

        ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

8.  設定使用者權限，以防止系統管理員帳戶以批次工作登入，方法是執行下列動作：

    1.  按兩下 [ **拒絕以批次工作登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

    3.  輸入 [ **系統管理員**]，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。 確認帳戶以 \Username 格式顯示， <DomainName> 如下列螢幕擷取畫面所示。

        ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

9. 設定使用者權限，以防止系統管理員帳戶以服務的身分登入，方法是執行下列動作：

    1.  按兩下 [ **拒絕以服務方式登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

    3.  輸入 [ **系統管理員**]，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。 確認帳戶以 \Username 格式顯示， <DomainName> 如下列螢幕擷取畫面所示。

        ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

10. 藉由執行下列動作，設定使用者權限以防止 BA 帳戶透過遠端桌面服務存取成員伺服器和工作站：

    1.  按兩下 [ **拒絕透過遠端桌面服務登** 入]，然後選取 [ **定義這些原則設定**]。

    2.  按一下 [ **新增使用者或群組** ]，然後按一下 **[流覽]**。

    3.  輸入 [ **系統管理員**]，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。 確認帳戶以 \Username 格式顯示， <DomainName> 如下列螢幕擷取畫面所示。

        ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)

    4.  按一下 **[確定]**，然後再按一下 **[確定]** 。

11. 若要結束**群組原則管理編輯器**，**請按一下 [** 檔案]，然後按一下 [結束 **]。**

12. 在 **群組原則管理**] 中，執行下列動作將 GPO 連結到成員伺服器和工作站 ou：

    1.  流覽至 <Forest> \Domains \\ <Domain> (其中 <Forest> 是樹系的名稱，而 <Domain> 是您要設定群組原則) 之網域的名稱。

    2.  以滑鼠右鍵按一下 GPO 將套用的 OU，然後按一下 [ **連結現有的 gpo**]。

        ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)

    3.  選取您所建立的 GPO，然後按一下 **[確定]**。

        ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)

    4.  建立包含工作站之所有其他 Ou 的連結。

    5.  建立包含成員伺服器的所有其他 Ou 的連結。

> [!IMPORTANT]
> 當您將系統管理員帳戶新增到這些設定時，您會指定要設定本機系統管理員帳戶或網域系統管理員帳戶，方法是將帳戶加上標籤的方式。 例如，若要將 TAILSPINTOYS 網域的系統管理員帳戶新增到這些拒絕許可權，您可以流覽至 TAILSPINTOYS 網域的系統管理員帳戶，此帳戶會顯示為 TAILSPINTOYS\Administrator。 如果您在群組原則物件編輯器的這些使用者權限設定中輸入「系統管理員」，您將會限制套用 GPO 之每部電腦上的本機系統管理員帳戶，如先前所述。

#### <a name="verification-steps"></a>驗證步驟
此處所述的驗證步驟是 Windows 8 和 Windows Server 2012 專用的。

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>確認「互動式登入需要智慧卡」帳戶選項

1.  從受 GPO 變更影響的任何成員伺服器或工作站，嘗試使用網域的內建系統管理員帳戶，以互動方式登入網域。 嘗試登入之後，應該會出現類似下列的對話方塊。

![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)

##### <a name="verify-account-is-disabled-account-option"></a>確認「帳戶已停用」帳戶選項

1.  從受 GPO 變更影響的任何成員伺服器或工作站，嘗試使用網域的內建系統管理員帳戶，以互動方式登入網域。 嘗試登入之後，應該會出現類似下列的對話方塊。

![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>確認「拒絕從網路存取這部電腦」 GPO 設定
從任何不受 GPO 影響的成員伺服器或工作站 (例如跳躍伺服器) ，嘗試透過受 GPO 變更影響的網路來存取成員伺服器或工作站。 若要確認 GPO 設定，請執行下列步驟來嘗試使用 **NET USE** 命令對應系統磁片磁碟機：

1.  使用網域內建的系統管理員帳戶登入網域。

2.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

3.  在 [ **搜尋** ] 方塊中，輸入 **命令提示**字元，以滑鼠右鍵按一下 [ **命令提示**字元]，然後按一下 [以 **系統管理員身分執行** ] 來開啟提升許可權的命令提示字元。

4.  當系統提示您核准提高許可權時，請按一下 **[是]**。

    ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)

5.  在 [**命令提示**字元] 視窗中，輸入**net use \\ \\ \<Server Name\> \c $**，其中 \<Server Name\> 是您嘗試透過網路存取的成員伺服器或工作站的名稱。

6.  下列螢幕擷取畫面顯示應出現的錯誤訊息。

    ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認「拒絕以批次工作登入」 GPO 設定

從受 GPO 變更影響的任何成員伺服器或工作站，在本機登入。

###### <a name="create-a-batch-file"></a>建立批次檔

1.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

2.  在 [ **搜尋** ] 方塊中，輸入 [ **記事本**]，然後按一下 [ **記事本**]。

3.  在 [ **記事本**] 中，輸入 **dir c：**。

4.  按一下 [檔案 **]，然後按一下 [** **另存**新檔]。

5.  在 [**檔案名**] 欄位中，輸入** <Filename> .bat** (其中 <Filename> 是) 的新批次檔的名稱。

###### <a name="schedule-a-task"></a>排程工作

1.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

2.  在 [ **搜尋** ] 方塊中 **，輸入 [** 工作排程器]，然後按一下 [ **工作排程器**]。

    > [!NOTE]
    > 在執行 Windows 8 的電腦上，于 [搜尋] 方塊中輸入 **排程**工作，然後按一下 [ **排程**工作]。

3.  在 **工作排程器**上，按一下 [ **動作**]，然後按一下 [ **建立**工作]。

4.  在 [ **建立** 工作] 對話方塊中，輸入 **<Task Name>** (其中 **<Task Name>** 是新工作) 的名稱。

5.  按一下 [ **動作** ] 索引標籤，然後按一下 [ **新增**]。

6.  在 [ **動作：**] 下，選取 [ **啟動程式**]。

7.  在 [ **程式/腳本：**] 下，按一下 **[流覽]**，找出並選取在 [建立批次檔] 區段中建立的批次檔，然後按一下 [ **開啟**]。

8.  按一下 [確定]。

9. 按一下 [General] \(一般\) 索引標籤。

10. 在 [ **安全性** 選項] 底下，按一下 [ **變更使用者或群組**]。

11. 在網域層級輸入 BA 帳戶的名稱，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

12. 選取 **[執行]，不論使用者是否登入** ，並 **不要儲存密碼**。 此工作將只能存取本機電腦資源。

13. 按一下 [確定]。

14. 應該會出現一個對話方塊，要求使用者帳號憑證來執行工作。

15. 輸入認證之後，請按一下 **[確定]**。

16. 應該會出現類似下列的對話方塊。

    ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認「拒絕以服務方式登入」 GPO 設定

1.  從受 GPO 變更影響的任何成員伺服器或工作站，在本機登入。

2.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

3.  在 [ **搜尋** ] 方塊中，輸入 **服務**，然後按一下 [ **服務**]。

4.  找出並按兩下 [ **列印多工緩衝處理器**]。

5.  单击 **“登录”** 选项卡。

6.  在 [ **登入為：**] 下，選取 [ **這個帳戶**]。

7.  按一下 **[流覽]**，在網域層級輸入 BA 帳戶的名稱，按一下 [ **檢查名稱**]，然後按一下 **[確定]**。

8.  在 [ **密碼：** ] 與 [ **確認密碼：**] 下，輸入系統管理員帳戶的密碼，然後按一下 **[確定]**。

9. 再按三次 **[確定]** 。

10. 以滑鼠右鍵按一下 [ **列印多工緩衝處理器] 服務** ，然後選取 [ **重新開機**]。

11. 當服務重新開機時，應該會出現類似下列的對話方塊。

    ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)

##### <a name="revert-changes-to-the-printer-spooler-service"></a>還原印表機多工緩衝處理器服務的變更

1.  從受 GPO 變更影響的任何成員伺服器或工作站，在本機登入。

2.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

3.  在 [ **搜尋** ] 方塊中，輸入 **服務**，然後按一下 [ **服務**]。

4.  找出並按兩下 [ **列印多工緩衝處理器**]。

5.  单击 **“登录”** 选项卡。

6.  在 [ **登入為：**] 下，選取 [ **本機系統** ] 帳戶，然後按一下 **[確定]**。

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>確認「拒絕登入遠端桌面服務」 GPO 設定

1.  使用滑鼠，將指標移至畫面的右上角或右下角。 當 **常用鍵** 列出現時，按一下 [ **搜尋**]。

2.  在 [ **搜尋** ] 方塊中，輸入 **remote desktop connection**，然後按一下 [ **遠端桌面連線**]。

3.  在 [ **電腦** ] 欄位中，輸入您要連線的電腦名稱稱，然後按一下 [連線 **]**。  (您也可以輸入 IP 位址，而不是電腦名稱稱。 ) 

4.  出現提示時，請在網域層級提供 BA 帳戶名稱的認證。

5.  應該會出現類似下列的對話方塊。

    ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)

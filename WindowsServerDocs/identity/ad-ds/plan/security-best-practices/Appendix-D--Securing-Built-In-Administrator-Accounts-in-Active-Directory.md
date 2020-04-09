---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: 附錄 D-在 Active Directory 中保護內建的系統管理員帳戶
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3e0060e4b732fe77de4371c7b84b77da21de9e20
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821661"
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>附錄 D︰保護 Active Directory 中的內建的 Administrator 帳戶

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>附錄 D︰保護 Active Directory 中的內建的 Administrator 帳戶  
在 Active Directory 中的每個網域中，系統會在建立網域的過程中建立系統管理員帳戶。 此帳戶預設為網域中 Domain Admins 和 Administrators 群組的成員，如果網域是樹系根域，則此帳戶也是 Enterprise Admins 群組的成員。

使用網域的系統管理員帳戶應僅保留給初始組建活動，以及可能發生的嚴重損壞修復案例。 若要確保在沒有其他帳戶可使用的情況下，可以使用系統管理員帳戶來影響修復，您不應該變更樹系中任何網域中系統管理員帳戶的預設成員資格。 相反地，您應該保護樹系中每個網域的系統管理員帳戶，如下一節所述，並在後續的逐步指示中詳述。 

> [!NOTE]
> 本指南是用來建議停用帳戶。 這已被移除，因為樹系復原技術白皮書會使用預設的系統管理員帳戶。 原因是，這是唯一可在沒有通用類別目錄伺服器的情況下進行登入的帳戶。


#### <a name="controls-for-built-in-administrator-accounts"></a>內建系統管理員帳戶的控制項  
對於樹系中每個網域內的內建 Administrator 帳戶，您應該設定下列設定：  

-   [啟用此**帳戶為機密]，而且無法**在帳戶上委派旗標。  

-   啟用帳戶上的**互動式登入旗標所需的智慧卡**。  

-   設定 Gpo 來限制系統管理員帳戶在已加入網域的系統上的使用：  

    -   在您建立的一或多個 Gpo 中，並連結到每個網域中的工作站和成員伺服器 Ou，請將每個網域的系統管理員帳戶新增至**電腦設定 \ \windows 設定 \ 使用者權限 \ 使用授權指派**中的下列使用者權利：  

        -   拒絕從網路存取這台電腦  

        -   拒絕以批次工作登入  

        -   拒絕以服務方式登入  

        -   拒絕透過遠端桌面服務登入  

> [!NOTE]  
> 當您將帳戶新增到此設定時，您必須指定要設定本機系統管理員帳戶或網域系統管理員帳戶。 例如，若要將 NWTRADERS 網域的系統管理員帳戶新增至這些拒絕許可權，您必須將該帳戶輸入為 NWTRADERS\Administrator，或流覽至 NWTRADERS 網域的系統管理員帳戶。 如果您在群組原則物件編輯器的這些使用者權限設定中輸入「系統管理員」，則會限制套用 GPO 的每部電腦上的本機系統管理員帳戶。
>   
> 建議您在成員伺服器和工作站上限制本機系統管理員帳戶，方法與網域系統管理員帳戶相同。 因此，您通常應該將樹系中每個網域的系統管理員帳戶，以及本機電腦的系統管理員帳戶新增至這些使用者權限設定。 下列螢幕擷取畫面顯示設定這些使用者權限以封鎖本機系統管理員帳戶的範例，以及網域的系統管理員帳戶，使其無法執行這些帳戶不需要的登入。  


![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   設定 Gpo 來限制網域控制站上的系統管理員帳戶  
    -   在樹系中的每個網域中，應修改預設網域控制站 GPO 或連結至網域控制站 OU 的原則，以將每個網域的系統管理員帳戶新增至**電腦設定 \ \windows 設定 \ 使用者權限 \ 許可權指派**中的下列使用者權利：   
        -   拒絕從網路存取這台電腦  

        -   拒絕以批次工作登入  

        -   拒絕以服務方式登入  

        -   拒絕透過遠端桌面服務登入  

> [!NOTE]  
> 這些設定會確保網域的內建 Administrator 帳戶無法用來連線到網域控制站，雖然帳戶（如果啟用的話）可以在本機登入網域控制站。 由於此帳戶應該只在嚴重損壞修復案例中啟用和使用，因此預期會有至少一個網域控制站的實體存取權，或可使用遠端存取網域控制站之許可權的其他帳戶。  

-   設定系統管理員帳戶的審核  

    當您已保護每個網域的系統管理員帳戶並予以停用時，您應該設定審核來監視帳戶的變更。 如果已啟用帳戶，則會重設其密碼，或對帳戶進行任何其他修改，並將警示傳送給負責管理 Active Directory 的使用者或小組，以及組織中的事件回應小組。  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>在 Active Directory 中保護內建的系統管理員帳戶的逐步指示  

1.  在**伺服器管理員**中，按一下 [**工具**]，然後按一下 [ **Active Directory 使用者和電腦**]。  

2.  若要防止利用委派在其他系統上使用帳號憑證的攻擊，請執行下列步驟：  

    1.  在**系統管理員**帳戶**上按一下滑鼠**右鍵，然後按一下 [內容]。  

    2.  按一下 [**帳戶**] 索引標籤。  

    3.  在 [**帳戶選項**] 底下，選取 [**帳戶是機密的，無法委派**] 旗標，如下列螢幕擷取畫面所示，然後按一下 **[確定]** 。  

        ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  若要啟用帳戶上的**互動式登入旗標所需的智慧卡**，請執行下列步驟：  

    1.  在**系統管理員**帳戶上按一下滑鼠右鍵，然後選取 [**屬性**]。  

    2.  按一下 [**帳戶**] 索引標籤。  

    3.  在 [**帳戶**選項] 底下，選取 [互動式登入] 旗標**所需的智慧卡**，如下列螢幕擷取畫面所示，然後按一下 **[確定]** 。  

        ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>設定 Gpo 以限制網域層級的系統管理員帳戶  

> [!WARNING]  
> 此 GPO 永遠不應連結到網域層級，因為它可以讓內建的系統管理員帳戶無法使用，即使在嚴重損壞修復案例中也一樣。  

1.  在**伺服器管理員**中，按一下 [**工具**]，然後按一下 [**群組原則管理**]。  

2.  在主控台樹中，展開 <Forest>\Domains\\<Domain>，然後**群組原則物件** （其中 <Forest> 是樹系的名稱，而 <Domain> 是您想要建立群組原則的網功能變數名稱稱）。  

3.  在主控台樹中，以滑鼠右鍵按一下 [**群組原則物件**]，然後按一下 [**新增**]。  

    ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  在 [**新增 GPO** ] 對話方塊中，輸入 <GPO Name>，然後按一下 **[確定]** （其中 <GPO Name> 是此 GPO 的名稱），如下列螢幕擷取畫面所示。  

    ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  在詳細資料窗格中，以滑鼠右鍵按一下 <GPO Name>，然後按一下 [**編輯**]。  

6.  流覽至 [**電腦設定 \ \windows] [設置 \ 本機原則**]，然後按一下 [**使用者權限指派**]。  

    ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  藉由執行下列動作，設定使用者權限以防止系統管理員帳戶存取成員伺服器和工作站：  

    1.  按兩下 **[拒絕從網路存取這部電腦**]，然後選取 [**定義這些原則設定**]。  

    2.  按一下 [**新增使用者或群組**]，然後按一下 **[流覽]** 。  

    3.  輸入**系統管理員**，按一下 [**檢查名稱**]，然後按一下 **[確定]** 。 確認帳戶以 <DomainName>\Username 格式顯示，如下列螢幕擷取畫面所示。  

        ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  按一下 **[確定]** ，然後再按一次 **[確定]** 。  

8.  設定使用者權限以防止系統管理員帳戶以批次工作登入，方法是執行下列動作：  

    1.  按兩下 [**拒絕以批次工作登**入]，然後選取 [**定義這些原則設定**]。  

    2.  按一下 [**新增使用者或群組**]，然後按一下 **[流覽]** 。  

    3.  輸入**系統管理員**，按一下 [**檢查名稱**]，然後按一下 **[確定]** 。 確認帳戶以 <DomainName>\Username 格式顯示，如下列螢幕擷取畫面所示。  

        ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  按一下 **[確定]** ，然後再按一次 **[確定]** 。  

9. 設定使用者權限以防止系統管理員帳戶以服務的身分登入，方法是執行下列動作：  

    1.  按兩下 [**拒絕以服務方式登**入]，然後選取 [**定義這些原則設定**]。  

    2.  按一下 [**新增使用者或群組**]，然後按一下 **[流覽]** 。  

    3.  輸入**系統管理員**，按一下 [**檢查名稱**]，然後按一下 **[確定]** 。 確認帳戶以 <DomainName>\Username 格式顯示，如下列螢幕擷取畫面所示。  

        ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  按一下 **[確定]** ，然後再按一次 **[確定]** 。  

10. 藉由執行下列動作，設定使用者權限以防止 BA 帳戶透過遠端桌面服務來存取成員伺服器和工作站：  

    1.  按兩下 [**拒絕透過遠端桌面服務登**入]，然後選取 [**定義這些原則設定**]。  

    2.  按一下 [**新增使用者或群組**]，然後按一下 **[流覽]** 。  

    3.  輸入**系統管理員**，按一下 [**檢查名稱**]，然後按一下 **[確定]** 。 確認帳戶以 <DomainName>\Username 格式顯示，如下列螢幕擷取畫面所示。  

        ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  按一下 **[確定]** ，然後再按一次 **[確定]** 。  

11. 若要結束**群組原則管理編輯器**，**請按一下 [** 檔案]，然後按一下 [結束 **]。**  

12. 在**群組原則管理** 中，執行下列動作，將 GPO 連結到成員伺服器和工作站 ou：  

    1.  流覽至 [<Forest>\Domains]\\<Domain> （其中 <Forest> 是樹系的名稱，而 [<Domain>] 是您要設定群組原則之網域的名稱）。  

    2.  以滑鼠右鍵按一下要套用 GPO 的 OU，然後按一下 [**連結到現有的 gpo**]。  

        ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  選取您建立的 GPO，然後按一下 **[確定]** 。  

        ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  建立包含工作站之所有其他 Ou 的連結。  

    5.  建立包含成員伺服器的所有其他 Ou 的連結。  

> [!IMPORTANT]  
> 當您將系統管理員帳戶新增至這些設定時，您可以指定是要依照如何為帳戶加上標籤來設定本機系統管理員帳戶或網域系統管理員帳戶。 例如，若要將 TAILSPINTOYS 網域的系統管理員帳戶新增至這些拒絕許可權，您可以流覽至 TAILSPINTOYS 網域的系統管理員帳戶，這會顯示為 TAILSPINTOYS\Administrator。 如果您在群組原則物件編輯器的這些使用者權限設定中輸入「系統管理員」，您將會限制套用 GPO 的每部電腦上的本機系統管理員帳戶，如先前所述。  

#### <a name="verification-steps"></a>驗證步驟  
這裡所述的驗證步驟僅適用于 Windows 8 和 Windows Server 2012。  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>確認「互動式登入需要智慧卡」帳戶選項  

1.  從任何受 GPO 影響的成員伺服器或工作站變更，嘗試使用網域內建的系統管理員帳戶，以互動方式登入網域。 嘗試登入之後，應該會出現類似下面的對話方塊。  

![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>確認 [帳戶已停用] 帳戶選項  

1.  從任何受 GPO 影響的成員伺服器或工作站變更，嘗試使用網域內建的系統管理員帳戶，以互動方式登入網域。 嘗試登入之後，應該會出現類似下面的對話方塊。  

![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>確認「拒絕從網路存取這部電腦」的 GPO 設定  
從任何不受 GPO 影響的成員伺服器或工作站（例如跳躍伺服器），嘗試透過受 GPO 變更的網路存取成員伺服器或工作站。 若要確認 GPO 設定，請執行下列步驟，嘗試使用**NET USE**命令來對應系統磁片磁碟機：  

1.  使用網域內建的系統管理員帳戶登入網域。  

2.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。  

3.  在**搜尋**方塊中，輸入**命令提示**字元，以滑鼠右鍵按一下 [**命令提示**字元]，然後按一下 [以**系統管理員身分執行**] 以開啟提升許可權的命令提示字元。  

4.  當系統提示您核准提高許可權時，按一下 **[是]** 。  

    ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  在 [**命令提示**字元] 視窗中，輸入**net use \\\\\<伺服器名稱\>\c $** ，其中 \<伺服器名稱\> 是您嘗試透過網路存取的成員伺服器或工作站的名稱。  

6.  下列螢幕擷取畫面顯示應該顯示的錯誤訊息。  

    ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認 [拒絕以批次工作登入] GPO 設定  

從任何受 GPO 影響的成員伺服器或工作站變更，在本機登入。  

###### <a name="create-a-batch-file"></a>建立批次檔  

1.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。  

2.  在**搜尋**方塊中，輸入**notepad**，然後按一下 [**記事本**]。  

3.  在 [**記事本**] 中，輸入**dir c：** 。  

4.  按一下 [檔案] **，然後按一下**[**另存**新檔]。  

5.  在 [**檔案名**] 欄位中，輸入 **<Filename>.bat** （其中 <Filename> 是新批次檔的名稱）。  

###### <a name="schedule-a-task"></a>排程工作  

1.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。  

2.  在 [**搜尋**] 方塊中 **，輸入 [** 工作排程器]，然後按一下 [**工作排程器**]。  

    > [!NOTE]  
    > 在執行 Windows 8 的電腦上，于 [搜尋] 方塊中輸入 [**排程**工作]，然後按一下 [**排程**工作]。  

3.  在**工作排程器**上，按一下 [**動作**]，然後按一下 [**建立**工作]。  

4.  在 [**建立**工作] 對話方塊中，輸入 **<Task Name>** （其中 **<Task Name>** 是新工作的名稱）。  

5.  按一下 [**動作**] 索引標籤，然後按一下 [**新增**]。  

6.  在 [**動作：** ] 底下，選取 [**啟動程式**]。  

7.  在 [**程式/腳本：** ] 底下，按一下 **[流覽]** ，找出並選取 [建立批次檔] 區段中建立的批次檔，然後按一下 [**開啟**]。  

8.  按一下 [確定]。  

9. 按一下 [一般] 索引標籤。  

10. 在 [**安全性**選項] 底下，按一下 [**變更使用者或群組**]。  

11. 在網域層級輸入 BA 帳戶的名稱，按一下 [**檢查名稱**]，然後按一下 **[確定]** 。  

12. 選取 **[執行] 表示使用者是否登入**，而且不會**儲存密碼**。 此工作只會存取本機電腦資源。  

13. 按一下 [確定]。  

14. 對話方塊應該會出現，要求使用者帳號憑證才能執行工作。  

15. 輸入認證之後，請按一下 **[確定]** 。  

16. 應該會出現類似下列的對話方塊。  

    ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認「拒絕以服務方式登入」的 GPO 設定  

1.  從任何受 GPO 影響的成員伺服器或工作站變更，在本機登入。  

2.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。  

3.  在 [**搜尋**] 方塊中，輸入**服務**，然後按一下 [**服務**]。  

4.  找出並按兩下 [**列印多工緩衝處理器**]。  

5.  单击 **“登录”** 选项卡。  

6.  在 [登入身分 **：** ] 底下，選取 [**此帳戶**]。  

7.  按一下 **[流覽]** ，在網域層級輸入 BA 帳戶的名稱，按一下 [**檢查名稱**]，然後按一下 **[確定]** 。  

8.  在 [**密碼：** ] 和 [**確認密碼：** ] 底下，輸入系統管理員帳戶的密碼，然後按一下 **[確定]** 。  

9. 再按三次 **[確定]** 。  

10. 以滑鼠右鍵按一下 [**列印多工緩衝處理器] 服務**，然後選取 [**重新開機**]。  

11. 重新開機服務時，應該會出現類似下列的對話方塊。  

    ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>還原對印表機多工緩衝處理程式服務所做的變更  

1.  從任何受 GPO 影響的成員伺服器或工作站變更，在本機登入。  

2.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。  

3.  在 [**搜尋**] 方塊中，輸入**服務**，然後按一下 [**服務**]。  

4.  找出並按兩下 [**列印多工緩衝處理器**]。  

5.  单击 **“登录”** 选项卡。  

6.  在 [登入身分 **：** ] 底下，選取 [**本機系統**] 帳戶，然後按一下 **[確定]** 。  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>確認「拒絕透過遠端桌面服務登入」 GPO 設定

1.  使用滑鼠，將指標移至畫面的右上方或右下角。 當**常用鍵**列出現時，按一下 [**搜尋**]。  

2.  在 [**搜尋**] 方塊中，輸入 [**遠端桌面**連線]，然後按一下 [**遠端桌面連線**]。  

3.  在 [**電腦**] 欄位中，輸入您要連線的電腦名稱稱，然後按一下 **[連線]** 。 （您也可以輸入 IP 位址，而不是電腦名稱稱。）  

4.  出現提示時，請在網域層級提供 BA 帳戶名稱的認證。  

5.  應該會出現類似下列的對話方塊。  

    ![保護內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  

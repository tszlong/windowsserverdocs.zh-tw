---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: 附錄 D-保護 Active Directory 中的內建的 Administrator 帳戶
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 51e503f55ee0fca1f1a53339de555fd213c69296
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870679"
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>附錄 D：保護 Active Directory 中的內建的 Administrator 帳戶

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>附錄 D：保護 Active Directory 中的內建的 Administrator 帳戶  
在 Active Directory 中的每個網域，系統管理員帳戶會建立為網域的一部分。 此帳戶在網域中，網域系統管理員和系統管理員群組的成員是依預設，如果網域樹系根網域，帳戶也是 Enterprise Admins 群組的成員。

使用網域系統管理員帳戶應保留只適用於初始建置活動，而且可能是嚴重損壞修復案例。 若要確保系統管理員帳戶，可用來影響修復，可以使用任何其他帳戶，您不應該變更預設的成員資格的樹系中任何網域中的系統管理員帳戶。 相反地，您應該保護樹系中每個網域中的系統管理員帳戶下, 一節中所述，並逐步指示，請依照下列所述。 

> [!NOTE]
> 建議停用帳戶使用本指南。 這已移除樹系復原技術白皮書會使用預設系統管理員帳戶。 原因是，這是唯一的帳戶，可讓不使用通用類別目錄伺服器的登入。


#### <a name="controls-for-built-in-administrator-accounts"></a>控制項的內建的 Administrator 帳戶  
在您的樹系中每個網域中的內建的系統管理員帳戶，您應該設定下列設定：  

-   啟用**是機密帳戶，無法委派**帳戶上的旗標。  

-   啟用**智慧卡是互動式登入必須**帳戶上的旗標。  

-   設定 Gpo 來限制在已加入網域的系統上的系統管理員帳戶的使用：  

    -   在您建立並連結至工作站和成員伺服器 Ou，每個網域中的一或多個 Gpo，加入每個網域系統管理員帳戶中的下列使用者權限**電腦設定 \ 原則 \windows 設定 \ 安全性設定 \ 本機原則 \ 使用者權限指派**:  

        -   拒絕從網路存取這台電腦  

        -   拒絕以批次工作登入  

        -   拒絕以服務方式登入  

        -   拒絕透過遠端桌面服務登入  

> [!NOTE]  
> 當您將帳戶加入此設定時，您必須指定您要設定本機系統管理員帳戶或網域系統管理員帳戶。 例如，將拒絕 NWTRADERS 網域的系統管理員帳戶，這些權限，您必須輸入帳戶 NWTRADERS\Administrator 或瀏覽 NWTRADERS 網域系統管理員帳戶。 如果您在這些使用者的權限設定中 群組原則物件編輯器 」 中輸入 「 系統管理員 」，您將會限制這個 GPO 會套用每一部電腦上的本機系統管理員帳戶。
>   
> 我們建議在網域系統管理員帳戶相同的方式來限制在成員伺服器和工作站上的本機系統管理員帳戶。 因此，您應該通常新增樹系中的每個網域的系統管理員帳戶和本機電腦的系統管理員帳戶對這些使用者的權限設定。 下列螢幕擷取畫面顯示範例設定這些來封鎖本機系統管理員帳戶和執行應該不需要為這些帳戶的登入的網域系統管理員帳戶的使用者權限。  


![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   設定 Gpo 來限制在網域控制站上的系統管理員帳戶  
    -   在樹系中每個網域，預設網域控制站 GPO 或原則連結應修改 OU，以將每個網域系統管理員帳戶新增至下列中的使用者權限的網域控制站**電腦設定 \ 原則 \windows\ 安全性設定 \ 本機原則 \ 使用者權限指派**:   
        -   拒絕從網路存取這台電腦  

        -   拒絕以批次工作登入  

        -   拒絕以服務方式登入  

        -   拒絕透過遠端桌面服務登入  

> [!NOTE]  
> 這些設定可確保連線到網域控制站，無法使用，網域的內建的系統管理員帳戶，雖然帳戶，如果啟用，可以登入本機網域控制站。 此帳戶應該僅啟用並用於災害復原案例，因為它預期會提供至少一個網域控制站的實體存取，或其他遠端存取網域控制站的權限的帳戶可以是使用此項目。  

-   設定稽核的系統管理員帳戶  

    當您有保護每個網域系統管理員帳戶，並將它停用時，您應該設定稽核來監視變更的帳戶。 如果已啟用的帳戶、 重設其密碼時，或任何其他修改的帳戶，應該會收到通知的使用者或小組負責管理 Active Directory 中，除了您組織中的事件回應小組。  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>保護 Active Directory 中的內建的 Administrator 帳戶的逐步指示  

1.  在**伺服器管理員**，按一下**工具**，然後按一下**Active Directory 使用者和電腦**。  

2.  若要避免運用委派給其他系統上使用的帳戶認證的攻擊，請執行下列步驟：  

    1.  以滑鼠右鍵按一下**系統管理員**帳戶，然後按一下**屬性**。  

    2.  按一下 [**帳戶**] 索引標籤。  

    3.  底下**帳戶選項**，選取**是機密帳戶，無法委派**旗標，如下列螢幕擷取畫面所示，然後按一下**確定**。  

        ![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  若要啟用**智慧卡是互動式登入必須**帳戶旗標，請執行下列步驟：  

    1.  以滑鼠右鍵按一下**系統管理員**帳戶，然後選取**屬性**。  

    2.  按一下 [**帳戶**] 索引標籤。  

    3.  底下**帳戶**選項中，選取**智慧卡是互動式登入必須**旗標，如下列螢幕擷取畫面所示，然後按一下**確定**。  

        ![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>設定 Gpo，來限制在網域層級的系統管理員帳戶  

> [!WARNING]  
> 永遠不會應將此 GPO 連結在網域層級，因為它可以讓內建的 Administrator 帳戶無法使用，即使在災害復原案例。  

1.  在 **伺服器管理員**，按一下**工具**，然後按一下**群組原則管理**。  

2.  在主控台樹狀目錄中，依序展開<Forest>\Domains\\<Domain>，然後**群組原則物件**(其中<Forest>樹系的名稱和<Domain>是您想要的網域名稱建立群組原則）。  

3.  在主控台樹狀目錄中，以滑鼠右鍵按一下**群組原則物件**，然後按一下**新增**。  

    ![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  中**新的 GPO**  對話方塊中，輸入<GPO Name>，然後按一下**確定** (其中<GPO Name>此 GPO 的名稱) 如下列螢幕擷取畫面所示。  

    ![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  在 [詳細資料] 窗格中，以滑鼠右鍵按一下<GPO Name>，然後按一下**編輯**。  

6.  瀏覽至**電腦設定 \ 原則 \windows 設定 \ 本機原則**，然後按一下**使用者權限指派**。  

    ![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  設定使用者權限，以防止系統管理員帳戶透過網路存取成員伺服器和工作站，執行下列動作：  

    1.  按兩下**拒絕從網路存取這台電腦**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

    3.  型別**系統管理員**，按一下**檢查名稱**，然後按一下**確定**。 確認顯示的帳戶時，會在<DomainName>\Username 格式如下列螢幕擷取畫面所示。  

        ![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  按一下  **確定**，並**確定**一次。  

8.  設定使用者權限，以防止系統管理員帳戶登入以批次工作執行下列動作：  

    1.  按兩下**拒絕以批次工作登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

    3.  型別**系統管理員**，按一下**檢查名稱**，然後按一下**確定**。 確認顯示的帳戶時，會在<DomainName>\Username 格式如下列螢幕擷取畫面所示。  

        ![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  按一下  **確定**，並**確定**一次。  

9. 設定使用者權限，以防止系統管理員帳戶登入為服務執行下列動作：  

    1.  按兩下**拒絕以服務登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

    3.  型別**系統管理員**，按一下**檢查名稱**，然後按一下**確定**。 確認顯示的帳戶時，會在<DomainName>\Username 格式如下列螢幕擷取畫面所示。  

        ![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  按一下  **確定**，並**確定**一次。  

10. 設定使用者權限，以防止 BA 帳戶透過下列方式存取成員伺服器和工作站，透過遠端桌面服務：  

    1.  按兩下**拒絕透過遠端桌面服務登入**，然後選取**定義這些原則設定**。  

    2.  按一下 **新增使用者或群組**然後按一下**瀏覽**。  

    3.  型別**系統管理員**，按一下**檢查名稱**，然後按一下**確定**。 確認顯示的帳戶時，會在<DomainName>\Username 格式如下列螢幕擷取畫面所示。  

        ![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  按一下  **確定**，並**確定**一次。  

11. 若要結束**群組原則管理編輯器**，按一下**檔案**，然後按一下**結束**。  

12. 在 **群組原則管理**，將 GPO 連結到成員伺服器和工作站的 Ou，執行下列動作：  

    1.  瀏覽至<Forest>\Domains\\ <Domain> (其中<Forest>樹系的名稱和<Domain>是您要將群組原則設定的網域名稱)。  

    2.  以滑鼠右鍵按一下 OU GPO 會套用至，然後按一下**連結到現有的 GPO**。  

        ![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  選取您所建立的 GPO，然後按一下**確定**。  

        ![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  建立連結至包含工作站的所有其他 Ou。  

    5.  建立連結至包含成員伺服器的所有其他 Ou。  

> [!IMPORTANT]  
> 當您新增的系統管理員帳戶，這些設定時，您可以指定設定的本機系統管理員帳戶或網域系統管理員帳戶由您設定帳戶的標籤。 例如，若要新增這些 TAILSPINTOYS 網域的系統管理員帳戶拒絕權限，您必須瀏覽至網域系統管理員帳戶 TAILSPINTOYS，這會顯示為 TAILSPINTOYS\Administrator。 如果您在這些使用者的權限設定中 群組原則物件編輯器 」 中輸入 「 系統管理員 」，您將會限制要套用 GPO，每一部電腦上的本機系統管理員帳戶，如先前所述。  

#### <a name="verification-steps"></a>驗證步驟  
此處所述的驗證步驟專屬於 Windows 8 和 Windows Server 2012。  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>確認 「 智慧卡 」 需要互動式登入帳戶選項  

1.  從任何成員伺服器或工作站 GPO 變更的影響，嘗試使用登入以互動方式加入網域的網域內建的 Administrator 帳戶。 在嘗試登入之後, 應該會出現如下所示的對話方塊。  

![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>確認 「 帳戶已停用 「 帳戶選項  

1.  從任何成員伺服器或工作站 GPO 變更的影響，嘗試使用登入以互動方式加入網域的網域內建的 Administrator 帳戶。 在嘗試登入之後, 應該會出現如下所示的對話方塊。  

![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>確認 「 拒絕從網路存取這台電腦 」 GPO 設定  
從任何成員伺服器或工作站 GPO 變更 （例如跳躍伺服器） 不會受到影響，嘗試存取的成員伺服器或工作站 GPO 變更的影響網路上。 若要確認 GPO 設定，請嘗試使用對應的系統磁碟機**NET USE**命令藉由執行下列步驟：  

1.  登入使用的網域內建的 Administrator 帳戶的網域。  

2.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

3.  在**搜尋**方塊中，輸入**命令提示字元**，以滑鼠右鍵按一下**命令提示字元**，然後按一下**系統管理員身分執行**開啟已提升權限命令提示字元。  

4.  當系統提示您核准提升權限，按一下**是**。  

    ![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  在 **命令提示字元**視窗中，輸入**net 使用\\ \\\<伺服器名稱\>\c$**，其中\<伺服器名稱\>是成員伺服器或您嘗試透過網路存取的工作站的名稱。  

6.  下列螢幕擷取畫面會顯示應該會出現錯誤訊息。  

    ![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>確認 「 拒絕登入以批次工作 」 GPO 設定  

從任何成員伺服器或工作站 GPO 變更的影響，在本機登入。  

###### <a name="create-a-batch-file"></a>建立批次檔  

1.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

2.  中**搜尋**方塊中，輸入**記事本**，然後按一下**記事本**。  

3.  在 **記事本**，型別**dir c:**。  

4.  按一下 **檔案**然後按一下**另存新檔**。  

5.  在 **檔名**欄位中，輸入 **<Filename>.bat** (其中<Filename>是新的批次檔的名稱)。  

###### <a name="schedule-a-task"></a>排程的工作  

1.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

2.  在 **搜尋**方塊中，輸入**工作排程器**，然後按一下**工作排程器**。  

    > [!NOTE]  
    > 在 [搜尋] 方塊中，執行 Windows 8 的電腦上輸入**排程工作**，然後按一下**排程工作**。  

3.  在上**工作排程器**，按一下**動作**，然後按一下**建立工作**。  

4.  在 [**建立工作**] 對話方塊中，輸入**<Task Name>** (其中**<Task Name>** 是新工作的名稱)。  

5.  按一下 **動作**索引標籤，然後按一下**新增**。  

6.  底下**動作：**，選取**啟動程式**。  

7.  底下**程式或指令碼：**，按一下**瀏覽**找出並選取 「 建立批次檔 」 一節中建立的批次檔，按一下 **開啟**。  

8.  按一下 [確定] 。  

9. 按一下 [一般] 索引標籤。  

10. 底下**安全性**選項，按一下**變更使用者或群組**。  

11. 輸入 BA 帳戶的名稱在網域層級，按一下 **檢查名稱**，然後按一下**確定**。  

12. 選取 **不論使用者登入與否均執行**並**不要儲存密碼**。 工作將只包含本機電腦資源的存取權。  

13. 按一下 [確定] 。  

14. 應該會出現一個對話方塊，要求的使用者帳戶的認證來執行工作。  

15. 輸入認證之後，按一下**確定**。  

16. 應該會出現如下所示的對話方塊。  

    ![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>確認 「 拒絕登入為服務 」 GPO 設定  

1.  從任何成員伺服器或工作站 GPO 變更的影響，在本機登入。  

2.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

3.  在 **搜尋**方塊中，輸入**服務**，然後按一下**服務**。  

4.  找出並按兩下**列印多工緩衝處理器**。  

5.  单击 **“登录”** 选项卡。  

6.  底下**身分登入：**，選取**此帳戶**。  

7.  按一下 **瀏覽**，輸入在網域層級的 BA 帳戶的名稱，按一下**檢查名稱**，然後按一下**確定**。  

8.  底下**密碼：** 並**確認密碼：**，輸入系統管理員帳戶的密碼，然後按一下**確定**。  

9. 按一下 **確定**三次。  

10. 以滑鼠右鍵按一下**列印多工緩衝處理器服務**，然後選取**重新啟動**。  

11. 重新啟動服務時，應該會出現如下所示的對話方塊。  

    ![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>還原的列印多工緩衝處理器服務的變更  

1.  從任何成員伺服器或工作站 GPO 變更的影響，在本機登入。  

2.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

3.  在 **搜尋**方塊中，輸入**服務**，然後按一下**服務**。  

4.  找出並按兩下**列印多工緩衝處理器**。  

5.  单击 **“登录”** 选项卡。  

6.  底下**身分登入：**，選取**本機系統**帳戶，再按一下**確定**。  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>確認 「 拒絕透過登入遠端桌面服務 」 GPO 設定

1.  使用滑鼠，請將指標移到螢幕的右上或右下角。 當**常用鍵**列出現時，按一下**搜尋**。  

2.  在 **搜尋**方塊中，輸入**遠端桌面連線**，然後按一下**遠端桌面連線**。  

3.  在 **電腦**欄位中，輸入您想要連接到，然後按一下 電腦名稱**Connect**。 （您也可以輸入的 IP 位址，而非電腦名稱）。  

4.  出現提示時，會提供認證 BA 帳戶在網域層級的名稱。  

5.  應該會出現如下所示的對話方塊。  

    ![保護的內建的系統管理員帳戶](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  

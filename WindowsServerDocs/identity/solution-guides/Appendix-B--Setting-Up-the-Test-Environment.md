---
ms.assetid: 82918181-525d-4e93-af96-957dac6aedb6
title: "B 附錄設定測試環境"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: deb08b0663e5f349df7cce51ddabd4aae7f624c5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-b-setting-up-the-test-environment"></a>B 附錄設定測試環境

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題描述建置手動實驗室測試動態存取控制的步驟。 依序有許多元件相依性，因為為了指示操作。  
  
## <a name="prerequisites"></a>必要條件  
**硬體與軟體需求**  
  
適用於設定實驗室測試需求：  
  
-   執行 Windows Server 2008 R2 SP1 與 HYPER-V 主機伺服器  
  
-   一份 Windows Server 2012 ISO  
  
-   一份 Windows 8 ISO  
  
-   Microsoft Office 2010  
  
-   執行 Microsoft Exchange Server 2003 或更新版本  
  
您需要組建測試案例動態存取控制下列虛擬電腦：  
  
-   DC1 （網域控制站）  
  
-   DC2 （網域控制站）  
  
-   1 （server 和 Active Directory Rights Management Services 檔案）  
  
-   SRV1 （POP3 和 SMTP 伺服器）  
  
-   CLIENT1 （client 電腦與 Microsoft Outlook）  
  
應該，如下所示虛擬電腦的密碼：  
  
-   BUILTIN\Administrator:pass@word1  
  
-   Contoso\Administrator:pass@word1  
  
-   所有其他帳號：pass@word1  
  
## <a name="build-the-test-lab-virtual-machines"></a>實驗室虛擬電腦組建測試  
  
### <a name="install-the-hyper-v-role"></a>安裝 HYPER-V 角色  
您需要執行 Windows Server 2008 R2 sp1 的電腦上安裝 HYPER-V 角色。  
  
##### <a name="to-install-the-hyper-v-role"></a>若要安裝 HYPER-V 角色  
  
1.  按一下**[開始]**，然後按一下 [伺服器管理員。  
  
2.  在伺服器管理員主要視窗的角色摘要區域中，按一下 [**新增角色**。  
  
3.  在**選取伺服器角色**頁面上，按**HYPER-V**。  
  
4.  在**建立 Virtual 網路**頁面上，按一下 [一或多個網路介面卡，如果您想要讓他們網路虛擬電腦使用。  
  
5.  在**確認安裝選項**頁面上，按**安裝**。  
  
6.  必須重新電腦啟動完成安裝。 按一下**關閉**以完成精靈中，然後按**是**電腦重新開機。  
  
7.  重新開機之後，使用的相同帳號，您用來安裝角色登入。 繼續設定精靈完成安裝之後，請按一下**關閉**以完成精靈。  
  
### <a name="create-an-internal-virtual-network"></a>建立 virtual 連絡  
您現在將會建立 virtual 連絡稱為 ID_AD_Network。  
  
##### <a name="to-create-a-virtual-network"></a>若要建立 virtual 網路  
  
1.  打開 HYPER-V 管理員。  
  
2.  從**動作**功能表上，按一下 [ **Virtual 的網路管理員]**。  
  
3.  在**建立 virtual 網路**、**內部**。  
  
4.  按一下**新增**。 **新 Virtual 網路**頁面隨即顯示。  
  
5.  輸入**ID_AD_Network**以在新的網路的名稱。 檢視其他屬性，並視進行修改。  
  
6.  按一下**[確定]**來建立 virtual 網路，並關閉 Virtual 的網路管理員] 中，或按**套用**建立 virtual 網路，並繼續使用 Virtual 的網路管理員。  
  
### <a name="BKMK_Build"></a>建立網域控制站  
組建一樣，做為網域控制站 (DC1)。 安裝一樣使用 Windows Server 2012 ISO，並為手機命名 DC1。  
  
##### <a name="to-install-active-directory-domain-services"></a>若要安裝 Active Directory Domain Services  
  
1.  連 ID_AD_Network 上一樣。 登入 DC1 以系統管理員密碼** pass@word1 **。  
  
2.  在伺服器管理員中，按一下**管理**，然後按**新增角色與功能**。  
  
3.  在**在您開始之前**頁面上，按一下 [**下**。  
  
4.  上**選取 [安裝類型**頁面上，按一下 [**以角色為基礎，或為基礎的功能的安裝**，然後按一下 [**下一步**。  
  
5.  在**選擇目的伺服器**頁面上，按一下 [**下**。  
  
6.  在**選擇伺服器角色**頁面上，按**Active Directory Domain Services**。 在**新增角色與功能精靈**對話方塊中，按**新增功能**，，然後按一下 [**下一步**。  
  
7.  在**選擇功能**頁面上，按一下 [**下**。  
  
8.  在**Active Directory Domain Services**頁面上，檢視資訊，然後按一下**下**。  
  
9. 在**確認安裝選項**頁面上，按**安裝**。 結果頁面上的功能安裝進度列指出正在安裝的角色。  
  
10. 在**結果**頁面上，確認已成功完成，再按一下安裝**關閉**。 在伺服器管理員中，按一下 [驚嘆號在右上角的畫面中，使用警告圖示旁邊**管理**。 在 [工作] 清單中，按一下**這個網域控制站伺服器升級**連結。  
  
11. 在**部署組態**頁面上，按一下 [**新增新的樹系**，輸入名稱根網域中， **contoso.com**，，然後按一下**下一步**。  
  
12. 在**網域控制站選項**頁面上，選取 [網域和森林功能等級與 Windows Server 2012，指定 DSRM 密碼** pass@word1 **，然後按一下 [**下一步**。  
  
13. 在**DNS 選項**頁面上，按**下**。  
  
14. 在**的其他選項**頁面上，按一下 [**下**。  
  
15. 在**路徑**頁面上，輸入 Active Directory 資料庫、 登入檔案，以及 SYSVOL 資料夾位置 （或接受預設的位置），然後按一下 [**下**。  
  
16. 在**評論選項**頁面，確認您的選項，然後按一下 [**下**。  
  
17. 在**請必要條件**頁面上，確認必要條件驗證完成，然後按一下 [**安裝**。  
  
18. 在**結果**頁面，確認為網域控制站伺服器已成功設定，然後按**關閉**。  
  
19. 重新開機才能完成 AD DS 安裝伺服器。 （根據預設，這會自動。）  
  
使用 Active Directory 管理中心建立下列使用者。  
  
##### <a name="create-users-and-groups-on-dc1"></a>DC1 建立使用者和群組  
  
1.  Contoso.com 以系統管理員身分登入。 上市 Active Directory 系統管理員中心。  
  
2.  建立下列安全性群組：  
  
    |群組的名稱|電子郵件地址|  
    |--------------|-----------------|  
    |FinanceAdmin|financeadmin@contoso.com|  
    |FinanceException|financeexception@contoso.com|  
  
3.  建立下列組織單位 （組織單位）：  
  
    |組織單位名稱|電腦|  
    |-----------|-------------|  
    |FileServerOU|1|  
  
4.  建立屬性，指定下列使用者：  
  
    |使用者|使用者名稱|電子郵件地址|部門|群組|國家/地區|  
    |--------|------------|-----------------|--------------|---------|-------------------|  
    |Myriam Delesalle|MDelesalle|MDelesalle@contoso.com|財經||我們|  
    |英哩 Reid|MReid|MReid@contoso.com|財經|FinanceAdmin|我們|  
    |Esther 耶|EValle|EValle@contoso.com|作業|FinanceException|我們|  
    |Maira Wenzel|MWenzel|MWenzel@contoso.com|小時||我們|  
    |Jeff 低|JLow|JLow@contoso.com|小時||我們|  
    |RMS 伺服器|rms|rms@contoso.com||||  
  
    如需有關如何建立安全性群組的詳細資訊，請[建立新群組](https://technet.microsoft.com/library/dd861305.aspx)Windows Server 網站上。  
  
##### <a name="to-create-a-group-policy-object"></a>若要建立群組原則物件  
  
1.  在右上角的 [螢幕上的游標，然後按一下 [搜尋] 圖示。 在搜尋方塊中，輸入**群組原則管理**，按一下 [**群組原則管理**。  
  
2.  展開**的樹系： contoso.com**，然後展開**網域**，瀏覽至**contoso.com**，展開 [ **(contoso.com)**，，然後選取 [ **FileServerOU**。 以滑鼠右鍵按一下**在這個網域中建立 GPO 並連結到**
  
3.  輸入描述性 GPO 的名稱，例如**FlexibleAccessGPO**，然後按**[確定]**。  
  
##### <a name="to-enable-dynamic-access-control-for-contosocom"></a>若要讓動態存取控制 contoso.com 的  
  
1.  打開群組原則管理主控台中，按一下**contoso.com**，然後按兩下 [**網域控制站**。  
  
2.  以滑鼠右鍵按一下**預設網域控制站原則**，然後選取 [**編輯**。  
  
3.  在群組原則編輯器] 管理視窗中，按兩下 [**電腦設定**，按兩下 [**原則**，按兩下 [**系統管理範本]**，按兩下**系統**，，然後按兩下 [ **KDC**。  
  
4.  按兩下**\ [KDC 支援宣告、 複合驗證以及 Kerberos 保護 \**下選取的選項**啟用**。 您必須支援此設定来使用的中央存取原則。  
  
5.  打開提升權限的命令提示字元中，執行下列命令：  
  
    ```  
    gpupdate /force  
    ```  
  
### <a name="BKMK_FS1"></a>組建檔案伺服器 AD RMS 伺服器 (1)  
  
1.  Windows Server 2012 ISO 從組建 1 名稱一樣。  
  
2.  連 ID_AD_Network 上一樣。  
  
3.  加入一樣 contoso.com 網域中，並再登入 1 contoso\administrator 使用密碼為** pass@word1 **。  
  
#### <a name="install-file-services-resource-manager"></a>安裝檔案服務資源管理員  
  
###### <a name="to-install-the-file-services-role-and-the-file-server-resource-manager"></a>若要安裝檔案服務角色與檔案伺服器資源管理員  
  
1.  在伺服器管理員中，按一下**新增角色與功能**。  
  
2.  在**在您開始之前**頁面上，按一下 [**下**。  
  
3.  在**選擇安裝類型**頁面上，按一下 [**下**。  
  
4.  在**選擇目的伺服器**頁面上，按一下 [**下**。  
  
5.  在**選取伺服器角色**頁面中，展開 [**檔案與儲存空間服務**，選取旁邊的核取方塊**檔案和 iSCSI 服務**，展開，然後選取 [**檔案伺服器資源管理員**。  
  
    新增角色與功能精靈中，按一下 [**新增功能**，然後按一下 [**下**。  
  
6.  在**選擇功能**頁面上，按一下 [**下**。  
  
7.  在**確認安裝選項**頁面上，按**安裝**。  
  
8.  在**安裝進度**頁面上，按**關閉**。  
  
#### <a name="install-the-microsoft-office-filter-packs-on-the-file-server"></a>該檔案伺服器上安裝 Microsoft Office 篩選器套件  
Windows Server 2012，以便 Ifilter 寬陣列超過預設所提供的 Office 檔案，您應該安裝 Microsoft Office 篩選套件。  Windows Server 2012 不需要任何 Ifilter 預設會安裝 Microsoft Office 檔案，檔案分類基礎結構使用 Ifilter 執行 content 分析。  
  
下載並安裝 Ifilter，請查看[Microsoft Office 2010 篩選套件](https://go.microsoft.com/fwlink/?LinkID=234122)。  
  
#### <a name="configure-email-notifications-on-file1"></a>設定電子郵件通知上 1  
當您建立配額和檔案畫面時，您可以選擇當他們事件即將或已被封鎖的檔案儲存嘗試進行後，使用者傳送電子郵件通知。 如果您想要定期通知特定的系統管理員配額和檢測事件檔案，您可以設定一或多個預設收件者。 若要傳送這些通知，您必須指定 SMTP 伺服器用於轉送的電子郵件訊息。  
  
###### <a name="to-configure-email-options-in-file-server-resource-manager"></a>設定電子郵件選項中檔案伺服器資源管理員  
  
1.  打開檔案伺服器資源管理員。 若要打開檔案伺服器資源管理員中，按一下 [ **[開始]**，輸入**檔案伺服器資源管理員**，然後按一下 [**檔案伺服器資源管理員**。  
  
2.  在檔案伺服器資源管理員介面，以滑鼠右鍵按一下**檔案伺服器資源管理員**，然後按一下 [**設定選項**。 **檔案伺服器資源管理員選項**對話方塊。  
  
3.  在**的電子郵件通知**索引標籤的 [SMTP 伺服器名稱或 IP 位址，輸入主機名稱將向前 SMTP 伺服器的 IP 位址電子郵件通知。  
  
4.  如果您想要定期通知特定的系統管理員配額的檔案或檢測活動，在**系統管理員收件者預設的**，輸入每個電子郵件地址，像是fileadmin@contoso.com。使用的格式account@domain，並使用分號來分隔多個帳號。  
  
#### <a name="create-groups-on-file1"></a>在 1 中建立群組  
  
###### <a name="to-create-security-groups-on-file1"></a>若要建立安全性群組 1  
  
1.  Contoso\administrator，使用密碼登入 1: ** pass@word1 **。  
  
2.  新增 NT 授權 \ 驗證使用者**WinRMRemoteWMIUsers__**群組。  
  
#### <a name="create-files-and-folders-on-file1"></a>1 上建立的檔案和資料夾  
  
1.  建立新的 NTFS 磁碟區上 1 並建立下列資料夾： D:\Finance 文件。  
  
2.  建立下列檔案指定的詳細資料：  
  
    -   **財務 Memo.docx**： 加入一些財務相關文件中的文字。 例如，' 商務規則的相關人員可以存取財經文件已變更。 財經文件現在只存取的 FinanceExpert 群組成員。 其他部門或群組存取。] 您需要評估的影響的前環境中執行這項變更。 請確定為每個頁面上的頁尾有 CONTOSO 機密這份文件。  
  
    -   **要求 Hire.docx 核准**： 建立表單本文件會收集申請人資訊。 您必須在文件下列欄位：**申請人名稱、 社會安全、 工作職稱、 建議薪資、 從日期、 監護人名稱部門**。 新增的表單的文件中的其他區段**監護人簽章，核准薪資，確認的提供**，以及**提供狀態**。   
        請讓文件版權管理。  
  
    -   **文字 Document1.docx**： 部分測試 content 新增這份文件。  
  
    -   **文字 Document2.docx**： 新增測試 content 這份文件。  
  
    -   **Workbook1.xlsx**  
  
    -   **Workbook2.xlsx**  
  
    -   在桌面上稱為規則運算式中建立資料夾。 建立文字文件在名為**RegEx-SSN**。 輸入下列 content 檔案，然後儲存，並關閉檔案：   
        ^(?!000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-] 嗎？)(?!00) \d\d\3 （？ ！\d {4} $ 0000)  
  
3.  共用 D:\Finance 文件以財經文件] 資料夾，並讓每個人都有讀取和寫入分享存取權。  
  
> [!NOTE]  
> 中央存取原則不支援預設系統或開機 c： 磁碟區。  
  
#### <a name="BKMK_CS1"></a>安裝 Active Directory Rights Management Services  
Active Directory Rights Management Services (AD RMS) 和所有所需的功能透過伺服器管理員中新增。 選擇您所有的預設值。  
  
###### <a name="to-install-active-directory-rights-management-services"></a>若要安裝 Active Directory Rights Management Services  
  
1.  以 CONTOSO\Administrator 或群組成員的網域系統管理員身分登入 1。  
  
    > [!IMPORTANT]  
    > 為了安裝 AD RMS 伺服器角色安裝程式 account （在本案例，CONTOSO\Administrator） 將會有提供群組成員資格同時本機系統管理員安裝所在 AD RMS 伺服器電腦上的，以及在 Active Directory 中企業系統管理員群組成員資格。  
  
2.  在伺服器管理員中，按一下**新增角色與功能**。 [新增角色與功能精靈會出現。  
  
3.  在**在您開始之前**畫面中，按一下 [**下**。  
  
4.  在**選擇安裝類型**畫面中，按一下 [**角色/功能型安裝**，，然後按一下 [**下一步**。  
  
5.  在**選擇伺服器目標**畫面中，按一下 [**下**。  
  
6.  在**選取伺服器角色**畫面中，選取核取方塊接下來**Active Directory Rights Management Services**，然後按一下 [**下一步**。  
  
7.  在**新增所需的 Active Directory Rights Management Services 功能？**對話方塊中，按一下 [ **[新增功能**。  
  
8.  在**選取伺服器角色**畫面中，按**下**。  
  
9. 在**選取要安裝的功能**畫面中，按**下**。  
  
10. 在**Active Directory Rights Management Services**畫面中，按一下 [下一步]。  
  
11. 在**選擇角色服務**畫面中，按一下 [**下**。  
  
12. 在**網頁伺服器角色 (IIS)**畫面中，按**下**。  
  
13. 在**選擇角色服務**畫面中，按一下 [**下**。  
  
14. 在**確認安裝選項**畫面中，按**安裝**。  
  
15. 安裝完成後，在後**安裝進度**畫面中，按**執行額外的設定**。 AD RMS 設定精靈會出現。  
  
16. 在**AD RMS**畫面中，按**下**。  
  
17. 在**AD RMS 叢集**畫面上，選取**建立新的 AD RMS 叢集根**，然後按一下 [**下一步**。  
  
18. 在**設定資料庫**畫面中，按一下 [**此伺服器上使用 Windows 內部資料庫**，然後按一下 [**下一步**。  
  
    > [!NOTE]  
    > 使用 Windows 內部資料庫建議的測試環境只因為它不支援 AD RMS 叢集在多部伺服器。 Production 部署應該使用不同的資料庫伺服器。  
  
19. 在**服務 Account**畫面上，在**使用者核對**，按一下 [**指定**，然後指定的使用者名稱 (**contoso\rms**)，和密碼 (**pass@word1**)，按一下 [ **[確定]**，，然後按一下 [**下一步**。  
  
20. 在**模式密碼編譯**畫面中，按**密碼編譯模式 2**。  
  
21. 在**叢集金鑰存放裝置**畫面中，按一下 [**下**。  
  
22. 在**叢集金鑰密碼**畫面上，在**密碼**和**確認密碼**方塊中，輸入** pass@word1 **，，然後按一下**下一步**。  
  
23. 在**網站叢集**畫面上，請確定**預設的網站**已選取，然後按一下 [**下一步**。  
  
24. 上**叢集地址**畫面上，選取**使用未加密的連接**選項，在**完整網域名稱**方塊中，輸入**FILE1.contoso.com**，然後按一下 [**下一步**。  
  
25. 在**授權憑證的名稱**畫面上，接受預設名稱 (**1**) 文字方塊中按一下 [**下一步**。  
  
26. 在**SCP 登記**畫面上，選取**登記 SCP 現在**，然後按一下 [**下一步**。  
  
27. 在**確認**畫面中，按**安裝**。  
  
28. 在**結果**畫面中，按一下 [**關閉**，然後按一下 [**關閉**上**安裝進度**螢幕。 完成時，請先登出，然後以 contoso\rms 使用密碼登入 (**pass@word1**)。  
  
29. 上市 AD RMS 主機並瀏覽至**權限原則範本**。  
  
    若要打開 AD RMS 主控台，在伺服器管理員中，按一下 [**本機伺服器**然後按一下 [主控台中**工具**，然後按一下 [ **Active Directory Rights Management Services**。  
  
30. 按一下**建立散發權利原則**範本位於右窗格中，按一下**新增**，然後選取下列資訊：  
  
    -   語言： 美式英文  
  
    -   名稱： Contoso 財經系統管理員  
  
    -   描述： Contoso 財經系統管理員  
  
    按一下**新增**，然後按一下 [**下**。  
  
31. 使用者與權限] 區段下，按一下 [**使用者與權限**，按一下 [**新增**，輸入** financeadmin@contoso.com **，按一下 [ **[確定]**。  
  
32. 選取 [**完全控制**，並保留**授與權限的任何的到期擁有者 （作者） 完全控制**選取。  
  
33. 按一下顯示的任何變更，剩餘的索引標籤，然後按一下**完成**。 CONTOSO\Administrator 的身分登入。  
  
34. 瀏覽至資料夾 C:\inetpub\wwwroot\\_wmcs\certification，選取 ServerCertification.asmx 檔案，並新增驗證使用者有讀取和寫入的檔案權限。  
  
35. 打開 Windows PowerShell 並執行`Get-FsrmRmsTemplate`。 確認您能以查看您在這個命令此程序的上一個步驟中建立 RMS 範本。  
  
> [!IMPORTANT]  
> 如果您想要立即變更，您可以將它們測試您的檔案伺服器，您需要執行下列動作：  
>   
> 1.  在檔案伺服器、 1，開放提升權限的命令提示字元中，執行下列命令：  
>   
>     -   gpupdate /force。  
>     -   NLTEST /SC_RESET:contoso.com  
> 2.  網域控制站 (DC1)，複寫 Active Directory。  
>   
>     如需強制複寫 Active Directory 步驟進行，查看[Active Directory 複寫](https://technet.microsoft.com/library/cc794809(WS.10).aspx)  
  
（選擇性） 而不是使用新增角色及功能精靈在伺服器管理員中，您可以使用 Windows PowerShell 來安裝和設定為下列程序中顯示的 AD RMS 伺服器角色。  
  
###### <a name="to-install-and-configure-an-ad-rms-cluster-in-windows-server-2012-using-windows-powershell"></a>安裝和使用 Windows PowerShell Windows Server 2012 中設定 AD RMS 叢集  
  
1.  使用密碼登上為 CONTOSO\Administrator: ** pass@word1 **。  
  
    > [!IMPORTANT]  
    > 為了安裝 AD RMS 伺服器角色安裝程式 account （在本案例，CONTOSO\Administrator） 將會有提供群組成員資格同時本機系統管理員安裝所在 AD RMS 伺服器電腦上的，以及在 Active Directory 中企業系統管理員群組成員資格。  
  
2.  伺服器桌面，請以滑鼠右鍵按一下工作列上選取 [Windows PowerShell 圖示**系統管理員身分執行**Windows PowerShell 命令提示字元打開的系統管理員權限。  
  
3.  若要使用 cmdlet 伺服器管理員安裝 AD RMS 伺服器角色，請輸入：  
  
    ```  
    Add-WindowsFeature ADRMS '"IncludeAllSubFeature '"IncludeManagementTools  
    ```  
  
4.  建立 Windows PowerShell 磁碟機來代表您要安裝的 AD RMS 伺服器。  
  
    例如，建立名為 RC 安裝和 AD RMS 根叢集中設定伺服器第一個 Windows PowerShell 磁碟機，請輸入：  
  
    ```  
    Import-Module ADRMS  
    New-PSDrive -PSProvider ADRMSInstall -Name RC -Root RootCluster  
    ```  
  
5.  設定命名空間的磁碟機中，表示需要的設定的物件的屬性。  
  
    例如，若要設定 AD RMS 服務帳號，Windows PowerShell 命令提示字元中，輸入：  
  
    ```  
    $svcacct = Get-Credential  
    ```  
  
    Windows 安全性對話方塊出現時，輸入 AD RMS 服務 account 網域使用者名稱 CONTOSO\RMS 及已指派的密碼。  
  
    接下來，若要指定 AD RMS 叢集設定 AD RMS 服務帳號，輸入下列動作：  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name ServiceAccount -Value $svcacct  
    ```  
  
    接下來，若要設定 AD RMS 伺服器的 Windows PowerShell 命令提示字元中，使用 Windows 內部資料庫，請輸入：  
  
    ```  
    Set-ItemProperty -Path RC:\ClusterDatabase -Name UseWindowsInternalDatabase -Value $true  
    ```  
  
    下一步] 安全地儲存在變數中，於 Windows PowerShell 命令提示字元中輸入的叢集金鑰密碼：  
  
    ```  
    $password = Read-Host -AsSecureString -Prompt "Password:"  
    ```  
  
    輸入叢集金鑰密碼，然後按 ENTER 鍵。  
  
    接下來，若要將密碼指派給您的 AD RMS 安裝，在 Windows PowerShell 命令提示字元中，請輸入：  
  
    ```  
    Set-ItemProperty -Path RC:\ClusterKey -Name CentrallyManagedPassword -Value $password  
    ```  
  
    接下來，若要設定 AD RMS 叢集地址，請在 Windows PowerShell 命令提示字元，請輸入：  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name ClusterURL -Value "http://file1.contoso.com:80"  
    ```  
  
    接下來，若要指派 SLC 名稱 AD RMS 安裝，在 Windows PowerShell 命令提示字元中，請輸入：  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name SLCName -Value "FILE1"  
    ```  
  
    接下來，若要設定 AD RMS 叢集、 服務連接點 (SCP) 的 Windows PowerShell 命令提示字元，請輸入：  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name RegisterSCP -Value $true  
    ```  
  
6.  執行**安裝-ADRMS** cmdlet。 除了安裝 AD RMS 伺服器角色設定伺服器，這個 cmdlet 也會安裝其他 AD RMS 必要時所需的功能。  
  
    例如，若要變更 Windows PowerShell 的磁碟機 RC 並安裝，設定 AD RMS 輸入：  
  
    ```  
    Set-Location RC:\  
    Install-ADRMS -Path.  
    ```  
  
    輸入 「 Y 「 時 cmdlet 會提示您確認您想要開始安裝。  
  
7.  CONTOSO\Administrator 和登入以登入 CONTOSO\RMS 使用提供的密碼 (」pass@word1「)。  
  
    > [!IMPORTANT]  
    > 為了管理 AD RMS 伺服器帳號，您的登入與管理的伺服器 （在本案例，CONTOSO\RMS） 使用必須指定這兩個本機系統管理員群組 AD RMS 伺服器的電腦，以及在 Active Directory 中企業系統管理員群組成員資格的資格。  
  
8.  伺服器桌面，請以滑鼠右鍵按一下工作列上選取 [Windows PowerShell 圖示**系統管理員身分執行**Windows PowerShell 命令提示字元打開的系統管理員權限。  
  
9. 建立 Windows PowerShell 磁碟機來表示您的設定 AD RMS 伺服器。  
  
    例如，以建立名稱為 RC 設定根 AD RMS 叢集 Windows PowerShell 磁碟機，請輸入：  
  
    ```  
    Import-Module ADRMSAdmin `  
    New-PSDrive -PSProvider ADRMSAdmin -Name RC -Root http://localhost -Force -Scope Global  
    ```  
  
10. 若要建立新的權限範本 Contoso 財經系統管理員，並將它指派使用者權利與完全控制 AD RMS 安裝，在 Windows PowerShell 命令提示字元中，輸入：  
  
    ```  
    New-Item -Path RC:\RightsPolicyTemplate '"LocaleName en-us -DisplayName "Contoso Finance Admin Only" -Description "Contoso Finance Admin Only" -UserGroup financeadmin@contoso.com  -Right ('FullControl')  
    ```  
  
11. 若要確認 Windows PowerShell 命令提示字元中，以 Contoso 財經系統管理員可以看到新的權限範本：  
  
    ```  
    Get-FsrmRmsTemplate  
    ```  
  
    目前檢視的下列 cmdlet 來確認您建立一個步驟中 RMS 範本輸出。  
  
### <a name="build-the-mail-server-srv1"></a>組建郵件伺服器 (SRV1)  
SRV1 是 SMTP 日 POP3 郵件伺服器。 您需要設定，讓您可以存取的協助案例的一部分傳送電子郵件通知。  
  
在這台電腦上設定 Microsoft Exchange Server。 如需詳細資訊，請查看[安裝 Exchange Server 如何](https://go.microsoft.com/fwlink/?prd=12364)。  
  
### <a name="build-the-client-virtual-machine-client1"></a>組建 client 一樣 (CLIENT1)  
  
##### <a name="to-build-the-client-virtual-machine"></a>建置 client 一樣  
  
1.  若要 ID_AD_Network 連接 CLIENT1。  
  
2.  安裝 Microsoft Office 2010。  
  
3.  Contoso\Administrator 的身分登入，請使用下列資訊來設定 Microsoft Outlook。  
  
    -   您的名稱︰ 檔案系統管理員  
  
    -   電子郵件地址：fileadmin@contoso.com  
  
    -   考慮類型： POP3  
  
    -   輸入電子郵件伺服器： SRV1 靜態 IP 位址  
  
    -   傳出郵件伺服器： SRV1 靜態 IP 位址  
  
    -   使用者名稱：fileadmin@contoso.com  
  
    -   記住密碼： 選取  
  
4.  建立 outlook contoso\administrator 桌面的快速鍵。  
  
5.  打開 Outlook 和地址所有 '啟動第一次 」 的訊息。  
  
6.  Delete 之任何測試訊息。  
  
7.  建立新的快顯 client 一樣指向 \\\FILE1\Finance 文件上所有使用者的桌面上。  
  
8.  視需要開機。  
  
##### <a name="enable-access-denied-assistance-on-the-client-virtual-machine"></a>讓 client 一樣存取的協助  
  
1.  打開作業系統，並瀏覽至**HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Explorer**。  
  
    -   設定**EnableShellExecuteFileStreamCheck**以**1**。  
  
    -   值： DWORD  
  
## <a name="BKMK_CF"></a>跨樹系案例部署宣告 lab 設定  
  
### <a name="BKMK_2.1"></a>DC2 的組建一樣  
  
-   Windows Server 2012 ISO 的組建一樣。  
  
-   建立 DC2 一樣名稱。  
  
-   連 ID_AD_Network 上一樣。  
  
> [!IMPORTANT]  
> 加入網域的電腦虛擬和部署宣告類型跨樹系需要虛擬的電腦無法解析相關網域的 Fqdn。 您可能需要手動虛擬完成此電腦上設定 DNS 設定。 如需詳細資訊，請查看[設定 virtual 網路](https://technet.microsoft.com/library/cc732470%28v=ws.10%29.aspx)。  
>   
> 所有一樣影像 （伺服器及戶端） 必須重都設使用版本 4 (IPv4) 靜態 IP 位址和網域名稱系統 」 (DNS) client 設定。 如需詳細資訊，請查看[適用於靜態 IP 位址設定 DNS Client](https://go.microsoft.com/fwlink/?LinkId=150952)。  
  
### <a name="BKMK_2.2"></a>新的樹系稱為 adatum.com 設定  
  
##### <a name="to-install-active-directory-domain-services"></a>若要安裝 Active Directory Domain Services  
  
1.  連 ID_AD_Network 上一樣。 登入 DC2 以系統管理員密碼** Pass@word1 **。  
  
2.  在伺服器管理員中，按一下**管理**，然後按**新增角色與功能**。  
  
3.  在**在您開始之前**頁面上，按一下 [**下**。  
  
4.  上**選取安裝類型**頁面上，按一下 [**以角色為基礎，或為基礎的功能的安裝**，然後按一下 [**下一步**。  
  
5.  在**選取目的伺服器**頁面上，按一下 [**選取伺服器伺服器集區的**，按一下您要安裝 Active Directory Domain Services (AD DS)，然後按一下 [伺服器名稱，**下一步**。  
  
6.  在**選取伺服器角色**頁面上，按**Active Directory Domain Services**。 在**新增角色與功能精靈**對話方塊中，按**新增功能**，，然後按一下 [**下一步**。  
  
7.  在**選擇功能**頁面上，按一下 [**下**。  
  
8.  在**AD DS**頁面上，檢視資訊，然後按一下**下**。  
  
9. 在**確認**頁面上，按**安裝**。 結果頁面上的功能安裝進度列指出正在安裝的角色。  
  
10. 在**結果**頁面上，確認安裝已成功完成，然後按一下 [驚嘆號右上角的畫面上的警告圖示旁邊**管理**。 在 [工作] 清單中，按一下**這個網域控制站伺服器升級**連結。  
  
    > [!IMPORTANT]  
    > 如果您關閉安裝精靈中，此時而不是按**這個網域控制站伺服器升級**，您可以按一下繼續 AD DS 安裝**工作**在伺服器管理員中。  
  
11. 在**部署組態**頁面上，按一下 [**新增新的樹系**，輸入名稱根網域中， **adatum.com**，，然後按一下**下一步**。  
  
12. 在**網域控制站選項**頁面上，選取 [網域和森林功能等級與 Windows Server 2012，指定 DSRM 密碼** pass@word1 **，然後按一下 [**下一步**。  
  
13. 在**DNS 選項**頁面上，按**下**。  
  
14. 在**的其他選項**頁面上，按一下 [**下**。  
  
15. 在**路徑**頁面上，輸入 Active Directory 資料庫、 登入檔案，以及 SYSVOL 資料夾位置 （或接受預設的位置），然後按一下 [**下**。  
  
16. 在**評論選項**頁面，確認您的選項，然後按一下 [**下**。  
  
17. 在**請必要條件**頁面上，確認必要條件驗證完成，然後按一下 [**安裝**。  
  
18. 在**結果**頁面，確認為網域控制站伺服器已成功設定，然後按**關閉**。  
  
19. 重新開機才能完成 AD DS 安裝伺服器。 （根據預設，這會自動。）  
  
> [!IMPORTANT]  
> 您必須以確保您設定的樹系之後網路已正常運作，設定，執行下列動作：  
>   
> -   Adatum\administrator 登入 adatum.com。 開放命令提示字元視窗中，輸入**nslookup contoso.com**，然後按 ENTER 鍵。  
> -   Contoso\administrator 登入 contoso.com。 開放命令提示字元視窗中，輸入**nslookup adatum.com**，然後按 ENTER 鍵。  
>   
> 如果不會出現錯誤，執行下列命令，森林可以彼此。 適用於 nslookup 錯誤的詳細資訊，請查看主題中的 [疑難排解] 區段[使用 NSlookup.exe](https://support.microsoft.com/kb/200525)  
  
### <a name="BKMK_2.22"></a>為信任的樹系 adatum.com 來設定 contoso.com  
在此步驟，您可以建立信任關係之間 Adatum Corporation 及 Contoso，ltd.網站。  
  
##### <a name="to-set-contoso-as-a-trusting-forest-to-adatum"></a>將 Contoso 設定為信任的樹系 Adatum 到  
  
1.  以系統管理員身分登入 DC2。 在**[開始]**畫面中，輸入 domain.msc。  
  
2.  在主控台 adatum.com，以滑鼠右鍵按一下，然後按一下屬性。  
  
3.  在**信任**索引標籤上，按一下 [**新增信任**，然後按一下 [**下一步**。  
  
4.  在**信任名稱**頁面上，輸入**contoso.com**，網域名稱系統 」 (DNS) 中的欄位，名稱，然後按**下一步**。  
  
5.  在**信任類型**頁面上，按一下 [**信任的樹系**，然後按一下 [**下一步**。  
  
6.  在**方向信任的**頁面上，按**雙向**。  
  
7.  上**信任側邊**頁面上，按一下 [**這兩個網域和指定的網域**，然後按一下 [**下一步**。  
  
8.  繼續依照精靈中的指示進行。  
  
### <a name="BKMK_2.4"></a>建立 Adatum 森林中的其他使用者  
建立 Jeff 低使用者使用密碼** pass@word1 **，並將指派公司屬性的值**Adatum**。  
  
##### <a name="to-create-a-user-with-the-company-attribute"></a>若要建立的使用者使用公司屬性  
  
1.  打開提升權限的命令提示字元中，Windows PowerShell 和貼上下列程式碼：  
  
    ```  
    New-ADUser `  
    -SamAccountName jlow `  
    -Name "Jeff Low" `  
    -UserPrincipalName jlow@adatum.com `  
    -AccountPassword (ConvertTo-SecureString `  
    -AsPlainText "pass@word1" -Force) `  
    -Enabled $true `  
    -PasswordNeverExpires $true `  
    -Path 'CN=Users,DC=adatum,DC=com' `  
    -Company Adatum`  
  
    ```  
  
### <a name="BKMK_2.5"></a>建立 adataum.com 公司宣告類型  
  
##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>若要使用 Windows PowerShell 來建立宣告類型  
  
1.  以系統管理員身分登入 adatum.com。  
  
2.  打開提升權限的命令提示字元中，Windows PowerShell 中，輸入下列程式碼：  
  
    ```  
    New-ADClaimType `  
    -AppliesToClasses:@('user') `  
    -Description:"Company" `  
    -DisplayName:"Company" `  
    -ID:"ad://ext/Company:ContosoAdatum" `  
    -IsSingleValued:$true `  
    -Server:"adatum.com" `  
    -SourceAttribute:Company `  
    -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("Contoso", "Contoso", "")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("Adatum", "Adatum", ""))) `  
  
    ```  
  
### <a name="BKMK_2.55"></a>讓 contoso.com 公司資源屬性  
  
##### <a name="to-enable-the-company-resource-property-on-contosocom"></a>若要讓 contoso.com 公司資源屬性  
  
1.  以系統管理員身分登入 contoso.com。  
  
2.  在伺服器管理員中，按一下**工具**，然後按**Active Directory 管理中心**。  
  
3.  在 Active Directory 管理中心的左窗格中，按一下 [**樹檢視**。 在左窗格中，按一下 [**動態存取控制**，然後按兩下 [**資源屬性**。  
  
4.  選取 [**公司**的**資源屬性**清單中，以滑鼠右鍵按一下，選取 [**屬性**。 在**建議值**區段中，按一下 [**新增**來新增建議的值： Contoso 和 Adatum，，然後按一下 [ **[確定]**兩次。  
  
5.  選取 [**公司**的**資源屬性**清單中，以滑鼠右鍵按一下，選取 [**可讓**。  
  
### <a name="BKMK_2.6"></a>讓 adatum.com 動態存取控制  
  
##### <a name="to-enable-dynamic-access-control-for-adatumcom"></a>若要讓動態存取控制 adatum.com 的  
  
1.  以系統管理員身分登入 adatum.com。  
  
2.  打開群組原則管理主控台中，按一下**adatum.com**，然後按兩下 [**網域控制站**。  
  
3.  以滑鼠右鍵按一下**預設網域控制站原則**，然後選取 [**編輯**。  
  
4.  在群組原則編輯器] 管理視窗中，按兩下 [**電腦設定**，按兩下 [**原則**，按兩下 [**系統管理範本]**，按兩下**系統**，，然後按兩下 [ **KDC**。  
  
5.  按兩下**\ [KDC 支援宣告、 複合驗證以及 Kerberos 保護 \**下選取的選項**啟用**。 您必須支援此設定来使用的中央存取原則。  
  
6.  打開提升權限的命令提示字元中，執行下列命令：  
  
    ```  
    gpupdate /force  
    ```  
  
### <a name="BKMK_2.8"></a>建立 contoso.com 公司宣告類型  
  
##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>若要使用 Windows PowerShell 來建立宣告類型  
  
1.  以系統管理員身分登入 contoso.com。  
  
2.  左提升權限的命令提示字元中 Windows PowerShell，然後輸入下列程式碼：  
  
    ```  
    New-ADClaimType '"SourceTransformPolicy `  
    '"DisplayName 'Company' `  
    '"ID 'ad://ext/Company:ContosoAdatum' `  
    '"IsSingleValued $true `  
    '"ValueType 'string' `  
  
    ```  
  
### <a name="BKMK_2.9"></a>建立的中央存取規則  
  
##### <a name="to-create-a-central-access-rule"></a>若要建立的中央存取規則  
  
1.  在 Active Directory 管理中心的左窗格中，按一下 [**樹檢視**。 在左窗格中，按一下 [**動態存取控制**，然後按**中央存取規則**。  
  
2.  以滑鼠右鍵按一下**中央存取規則**，按一下 [**新**，然後**中央存取規則**。  
  
3.  在**名稱**欄位中，輸入**AdatumEmployeeAccessRule**。  
  
4.  在**權限**區段中，選取**為目前的權限的權限之後使用**選項，請按一下 [**編輯**，，然後按一下**新增**。 按一下**選取主體**連結，輸入**Authenticated Users**，然後按一下 [ **[確定]**。  
  
5.  在**權限的項目權限的**對話方塊中，按一下 [ **[新增條件**，輸入下列條件: [**使用者**] [**公司**] [**等於**] [**值**] [**Adatum**]。 應該權限]**修改、 讀取並執行、 朗讀、 寫入**。  
  
6.  按一下**[確定]**。  
  
7.  按一下**[確定]**以完成，並回到 Active Directory 管理中心三次。  
  
    ![方案指南](media/Appendix-B--Setting-Up-the-Test-Environment/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *  
  
    下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。  
  
    ```  
    New-ADCentralAccessRule `  
    -CurrentAcl:"O:SYG:SYD:AR(A;;FA;;;OW)(A;;FA;;;BA)(A;;FA;;;SY)(XA;;0x1301bf;;;AU;(@USER.ad://ext/Company:ContosoAdatum == `"Adatum`"))" `  
    -Name:"AdatumEmployeeAccessRule" `  
    -ProposedAcl:$null `  
    -ProtectedFromAccidentalDeletion:$true `  
    -Server:"contoso.com" `  
    ```  
  
### <a name="BKMK_2.10"></a>建立的中央存取原則  
  
##### <a name="to-create-a-central-access-policy"></a>若要建立的中央存取原則  
  
1.  以系統管理員身分登入 contoso.com。  
  
2.  打開提升權限的命令提示字元中，Windows PowerShell 中，將下列程式碼：  
  
    ```  
    New-ADCentralAccessPolicy "Adatum Only Access Policy"   
    Add-ADCentralAccessPolicyMember "Adatum Only Access Policy" `  
    -Member "AdatumEmployeeAccessRule" `  
    ```  
  
### <a name="BKMK_2.11"></a>發行新原則透過群組原則  
  
##### <a name="to-apply-the-central-access-policy-across-file-servers-through-group-policy"></a>若要透過群組原則檔案伺服器上套用的中央存取原則  
  
1.  在**[開始]**畫面中，輸入**系統管理工具]**，並在**搜尋**列中，按一下**設定**。 在**設定**結果中，按**系統管理工具]**。 打開從 「 群組原則管理主控台**系統管理工具]**資料夾。  
  
    > [!TIP]  
    > 如果**顯示系統管理工具]**已停用設定、 [系統管理工具] 資料夾和內容不會出現在**設定**的結果。  
  
2.  以滑鼠右鍵按一下 contoso.com 網域中，按一下**在這個網域中建立 GPO 並連結到**  
  
3.  輸入描述性 GPO 的名稱，例如**AdatumAccessGPO**，然後按**[確定]**。  
  
##### <a name="to-apply-the-central-access-policy-to-the-file-server-through-group-policy"></a>若要套用的中央存取原則檔案伺服器透過群組原則  
  
1.  在**[開始]**畫面中，輸入**群組原則管理**，請在**搜尋**方塊。 開放**群組原則管理**的 [系統管理工具] 資料夾。  
  
    > [!TIP]  
    > 如果**顯示系統管理工具]**已停用設定、 [系統管理工具] 資料夾和內容不會出現在 [設定] 結果。  
  
2.  瀏覽並選取 Contoso，如下所示： 群組原則 Management\Forest: contoso.com\Domains\contoso.com。  
  
3.  以滑鼠右鍵按一下**AdatumAccessGPO**原則，然後選取**編輯**。  
  
4.  群組原則管理編輯器] 中，按一下 [**電腦設定**，展開**原則**，展開**Windows 設定**，然後按一下**的安全性設定**。  
  
5.  展開**檔案系統**，以滑鼠右鍵按一下**的中央存取原則**，然後按一下 [**管理中央存取原則**。  
  
6.  在**中央存取原則設定**對話方塊中，按一下 [**新增**、 選取**Adatum 只存取原則**，，然後按一下 [ **[確定]**。  
  
7.  關閉 「 群組原則管理編輯器。 您現在已群組原則來新增的中央存取原則。  
  
### <a name="BKMK_2.12"></a>獲利伺服器上建立資料夾的檔案  
建立新的 NTFS 磁碟區 1，並建立下列資料夾： D:\Earnings。  
  
> [!NOTE]  
> 中央存取原則不支援預設系統或開機 c： 磁碟區。  
  
### <a name="BKMK_2.13"></a>設定分類，並套用獲利資料夾中的中央存取原則  
  
##### <a name="to-assign-the-central-access-policy-on-the-file-server"></a>將檔案伺服器的中央存取原則  
  
1.  在 [HYPER-V 管理員連接伺服器 1。 Contoso\Administrator，使用密碼登入伺服器** pass@word1 **。  
  
2.  打開提升權限的命令提示字元中，輸入： **gpupdate /force**。 這樣可確保群組原則變更會影響您的伺服器。  
  
3.  您也需要重新整理全球 Active directory 資源屬性。 開放的 Windows PowerShell，輸入`Update-FSRMClassificationpropertyDefinition`，然後按 ENTER 鍵。 關閉 Windows PowerShell。  
  
4.  打開 Windows 檔案總管]，並瀏覽至 D:\EARNINGS。 以滑鼠右鍵按一下**獲利**資料夾，然後按**屬性**。  
  
5.  按一下**分類**索引標籤，選取**公司**，然後選取**Adatum**中**值**欄位。  
  
6.  按一下**變更**，請選取**Adatum 只存取原則**從下拉式功能表，然後按一下 [**套用]**。  
  
7.  按一下**安全性**索引標籤上，按一下 [**進階**，然後按一下 [**中央原則**索引標籤。您應該會看到**AdatumEmployeeAccessRule**列。 您可以展開以檢視所有的設定建立規則 Active Directory 中的權限的項目。  
  
8.  按一下**[確定]**以返回 [Windows 檔案總管]。  
  



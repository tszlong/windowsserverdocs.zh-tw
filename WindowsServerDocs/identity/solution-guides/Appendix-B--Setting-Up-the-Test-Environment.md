---
ms.assetid: 82918181-525d-4e93-af96-957dac6aedb6
title: 附錄 B 設定測試環境
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5f529e6b0176b7ad416a728163b4ae9671040bf8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861281"
---
# <a name="appendix-b-setting-up-the-test-environment"></a>附錄 B：設定測試環境

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題概述如何建置實際操作實驗室來測試動態存取控制的步驟。 文中的指示應該依序操作，因為許多元件具有相依性。  

## <a name="prerequisites"></a>必要條件  
**硬體和軟體需求**  

設定測試實驗室的需求：  

-   執行 Windows Server 2008 R2 SP1 與 Hyper-V 的主機伺服器  

-   Windows Server 2012 ISO 的複本  

-   Windows 8 ISO 的複本  

-   Microsoft Office 2010  

-   執行 Microsoft Exchange Server 2003 或更新版本的伺服器  

您必須建立下列虛擬機器，以測試動態存取控制的案例：  

-   DC1 (網域控制站)  

-   DC2 (網域控制站)  

-   FILE1 (檔案伺服器與 Active Directory Rights Management Services)  

-   SRV1 (POP3 和 SMTP 伺服器)  

-   CLIENT1 (安裝了 Microsoft Outlook 的用戶端電腦)  

虛擬機器的密碼應該如下所示：  

-   BUILTIN\Administrator： pass@word1  

-   Contoso\Administrator： pass@word1  

-   所有其他帳戶： pass@word1  

## <a name="build-the-test-lab-virtual-machines"></a>建置測試實驗室的虛擬機器  

### <a name="install-the-hyper-v-role"></a>安裝 Hyper-V 角色  
您需要在執行 Windows Server 2008 R2 SP1 的電腦上安裝 Hyper-V 角色。  

##### <a name="to-install-the-hyper-v-role"></a>安裝 Hyper-V 角色  

1.  按一下 [開始]，然後按一下 [伺服器管理員]。  

2.  在 [伺服器管理員] 主視窗的 [角色摘要] 區域中，按一下 [新增角色]。  

3.  在 [選取伺服器角色] 頁面上，按 [Hyper-V]。  

4.  在 [建立虛擬網路] 頁面上，按一下一或多個網路介面卡 (如果您想要讓虛擬機器使用其網路連線)。  

5.  在 [確認安裝選項] 頁面上，按一下 [安裝]。  

6.  您必須重新啟動電腦，才能完成安裝。 按一下 [關閉] 以完成精靈，然後按一下 [是] 重新啟動電腦。  

7.  重新啟動電腦之後，使用您用來安裝角色的相同帳戶登入。 [繼續設定精靈] 完成安裝之後，按一下 [關閉] 完成精靈。  

### <a name="create-an-internal-virtual-network"></a>建立內部的虛擬網路  
現在您將建立稱為 ID_AD_Network 的內部虛擬網路。  

##### <a name="to-create-a-virtual-network"></a>建立虛擬網路  

1.  開啟 \[Hyper-V 管理員\]。  

2.  從 [動作] 功能表上，按一下 [虛擬網路管理員]。  

3.  在 [建立虛擬網路] 之下，選取 [內部]。  

4.  按一下 [加入]。 [新的虛擬網路] 頁面隨即顯示。  

5.  輸入 **ID_AD_Network** 做為新網路的名稱。 檢閱其他內容，並視需要修改。  

6.  按一下 [確定] 來建立虛擬網路並關閉 [虛擬網路管理員]，或按一下 [套用] 來建立虛擬網路並繼續使用 [虛擬網路管理員]。  

### <a name="build-the-domain-controller"></a><a name="BKMK_Build"></a>建立網域控制站  
建立要做為網域控制站 (DC1) 的虛擬機器。 使用 Windows Server 2012 ISO 安裝虛擬機器，並將它命名為 DC1。  

##### <a name="to-install-active-directory-domain-services"></a>安裝 Active Directory 網域服務  

1. 將虛擬機器連線到 ID_AD_Network。 以系統管理員身分登入 DC1，並使用密碼<strong>pass@word1</strong>。  

2. 在 [伺服器管理員] 中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。  

3. 在 [在您開始前] 頁面中按 [下一步]。  

4. 在 [選取安裝類型] 頁面上，按一下 [角色型或功能型安裝]，然後按一下 [下一步]。  

5. 在 [選取目的地伺服器] 頁面，按 [下一步]。  

6. 在 [選取伺服器角色] 頁面上，選取 [Active Directory 網域服務]。 在 [新增角色及功能精靈] 對話方塊中，按一下 [新增功能]，然後按 [下一步]。  

7. 在 [選取功能] 頁面上，按 [下一步]。  

8. 檢閱 [Active Directory 網域服務] 頁面上提供的資訊，然後按一下 [下一步]。  

9. 在 [確認安裝選項] 頁面上，按一下 [安裝]。 [結果] 頁面上的功能安裝進度列表示正在安裝角色。  

10. 在 [結果] 頁面上，確認安裝成功，然後按一下 [關閉]。 在 [伺服器管理員] 中，按一下螢幕右上角 [管理] 旁邊的驚嘆號警告圖示。 在 [工作] 清單中，按一下 [將此伺服器升級為網域控制站] 連結。  

11. 在 [部署設定] 頁面上，按一下 [新增新的樹系]，輸入根網域的名稱 **contoso.com**，然後按一下 [下一步]。  

12. 在 [**網域控制站選項**] 頁面上，選取網域和樹系功能等級為 Windows Server 2012， <strong>pass@word1</strong>指定 DSRM 密碼，然後按 **[下一步]** 。  

13. 在 [DNS 選項] 頁面上，按一下 [下一步]。  

14. 在 [其他選項] 頁面上，按一下 [下一步]。  

15. 在 [路徑] 頁面上，輸入 Active Directory 資料庫、記錄檔以及 SYSVOL 資料夾的位置 (或接受預設位置)，然後按一下 [下一步]。  

16. 在 [檢閱選項] 頁面上，確認您的選項，然後按一下 [下一步]。  

17. 在 [先決條件檢查] 頁面上，確認先決條件驗證已完成，然後按一下 [安裝]。  

18. 在 [結果] 頁面上，確認伺服器已成功設定為網域控制站，然後按一下 [關閉]。  

19. 重新啟動伺服器，以完成 AD DS 安裝。 (預設會自動重新啟動。)  

使用 Active Directory 管理中心建立下列使用者。  

##### <a name="create-users-and-groups-on-dc1"></a>在 DC1 上建立使用者和群組  

1. 以系統管理員身分登入 contoso.com。 啟動 Active Directory 管理中心。  

2. 建立下列安全性群組：  


   |    群組名稱    |        電子郵件地址         |
   |------------------|------------------------------|
   |   FinanceAdmin   |   financeadmin@contoso.com   |
   | FinanceException | financeexception@contoso.com |


3. 建立下列組織單位 (OU)：  


   |   OU 名稱    | 電腦 |
   |--------------|-----------|
   | FileServerOU |   FILE1   |


4. 建立下列包含指定屬性的使用者：  


   |       使用者       |  Username  |     電子郵件地址      | 部門 |      群組       | Country/Region |
   |------------------|------------|------------------------|------------|------------------|----------------|
   | Myriam Delesalle | MDelesalle | MDelesalle@contoso.com |  財務   |                  |       美式英文       |
   |    Miles Reid    |   MReid    |   MReid@contoso.com    |  財務   |   FinanceAdmin   |       美式英文       |
   |   Esther Valle   |   EValle   |   EValle@contoso.com   | 操作 | FinanceException |       美式英文       |
   |   Maira Wenzel   |  MWenzel   |  MWenzel@contoso.com   |     HR     |                  |       美式英文       |
   |     Jeff Low     |    JLow    |    JLow@contoso.com    |     HR     |                  |       美式英文       |
   |    RMS 伺服器    |    rms     |    rms@contoso.com     |            |                  |                |

   如需建立安全性群組的詳細資訊，請參閱 Windows Server 網站上的 [建立新的群組](https://technet.microsoft.com/library/dd861305.aspx) 。  

##### <a name="to-create-a-group-policy-object"></a>建立群組原則物件  

1.  將游標停留在螢幕的右上角，然後按一下 [搜尋] 圖示。 在搜尋方塊中，輸入 [群組原則管理]，按一下 [群組原則管理]。  

2.  展開 [樹系: contoso.com]，然後展開 [網域]，瀏覽至 **contoso.com**，展開 [(contoso.com)]，然後選取 [FileServerOU]。 以滑鼠右鍵按一下 [**在這個網域中建立 GPO 並連結到**]

3.  輸入 GPO 的描述性名稱，例如 **FlexibleAccessGPO**，然後按一下 [確定]。  

##### <a name="to-enable-dynamic-access-control-for-contosocom"></a>啟用 contoso.com 的動態存取控制  

1.  開啟 [群組原則管理主控台]，按一下 [contoso.com]，然後按兩下 [網域控制站]。  

2.  以滑鼠右鍵按一下 [預設網域控制站原則]，然後選取 [編輯]。  

3.  在 [群組原則管理編輯器] 視窗中，依序按兩下 [電腦設定]、[原則]、[系統管理範本]、[系統]、[KDC]。  

4.  按兩下 [宣告、複合驗證和 Kerberos 防護的 KDC 支援] 並選取 [啟用] 旁邊的選項。 您必須啟用這個設定，才能使用集中存取原則。  

5.  開啟提升權限的命令提示字元，然後執行下列命令：  

    ```  
    gpupdate /force  
    ```  

### <a name="build-the-file-server-and-ad-rms-server-file1"></a><a name="BKMK_FS1"></a>建立檔案伺服器和 AD RMS 伺服器（FILE1）  

1. 從 Windows Server 2012 ISO 建立名為 FILE1 的虛擬機器。  

2. 將虛擬機器連線到 ID_AD_Network。  

3. 將虛擬機器加入 contoso.com 網域，然後使用密碼<strong>pass@word1</strong>登入 FILE1 作為 contoso\administrator。  

#### <a name="install-file-services-resource-manager"></a>安裝檔案服務資源管理員  

###### <a name="to-install-the-file-services-role-and-the-file-server-resource-manager"></a>安裝檔案服務角色與檔案伺服器資源管理員  

1.  在 [伺服器管理員] 中，按一下 [新增角色及功能]。  

2.  在 [在您開始前] 頁面中按 [下一步]。  

3.  在 [選取安裝類型] 頁面上，按一下 [下一步]。  

4.  在 [選取目的地伺服器] 頁面，按 [下一步]。  

5.  在 [選取伺服器角色] 頁面上，展開 [檔案和存放服務]，選取 [檔案與 iSCSI 服務] 旁邊的核取方塊，展開並選取 [檔案伺服器資源管理員]。  

    在 [新增角色及功能精靈] 中，按一下 [新增功能]，然後按一下 [下一步]。  

6.  在 [選取功能] 頁面上，按 [下一步]。  

7.  在 [確認安裝選項] 頁面上，按一下 [安裝]。  

8.  在 [安裝進度] 頁面上，按一下 [關閉]。  

#### <a name="install-the-microsoft-office-filter-packs-on-the-file-server"></a>在檔案伺服器上安裝 Microsoft Office Filter Pack  
您應該在 Windows Server 2012 上安裝 Microsoft Office 篩選器套件，以啟用比預設提供更多的 Office 檔案陣列的 Ifilter。  根據預設，Windows Server 2012 沒有安裝 Microsoft Office 檔案的任何 Ifilter，而且檔案分類基礎結構會使用 Ifilter 來執行內容分析。  

若要下載及安裝 IFilter，請參閱 [Microsoft Office 2010 篩選套件](https://go.microsoft.com/fwlink/?LinkID=234122)。  

#### <a name="configure-email-notifications-on-file1"></a>在 FILE1 上設定電子郵件通知  
建立配額和檔案檢測時，您可以選擇在使用者接近限制配額時或使用者嘗試儲存已被封鎖的檔案之後，傳送電子郵件通知他們。 如果您想要定期通知處理配額與檔案檢測事件的特定系統管理員，您可以設定一或多個預設收件者。 若要傳送這些通知，您必須指定用於轉送電子郵件的 SMTP 伺服器。  

###### <a name="to-configure-email-options-in-file-server-resource-manager"></a>設定檔案伺服器資源管理員的電子郵件選項  

1. 開啟檔案伺服器資源管理員。 若要開啟檔案伺服器資源管理員，按一下 [開始]，輸入**檔案伺服器資源管理員**，然後按一下 [檔案伺服器資源管理員]。  

2. 在 [檔案伺服器資源管理員] 介面中，以滑鼠右鍵按一下 [檔案伺服器資源管理員]，然後按一下 [設定選項]。 [檔案伺服器資源管理員選項] 對話方塊隨即開啟。  

3. 在 [電子郵件通知] 索引標籤的 SMTP 伺服器名稱或 IP 位址之下，輸入要用來轉送電子郵件通知的 SMTP 伺服器的主機名稱或 IP 位址。  

4. 如果您想要定期通知特定管理員配額或檔案檢測事件，請在 [**預設系統管理員**收件者] 下，輸入每個電子郵件地址，例如 fileadmin@contoso.com。 請使用 account@domain格式，並使用分號來分隔多個帳戶。  

#### <a name="create-groups-on-file1"></a>在 FILE1 上建立群組  

###### <a name="to-create-security-groups-on-file1"></a>在 FILE1 上建立安全性群組  

1. 以 contoso\administrator 的身分登入 FILE1，其密碼為： <strong>pass@word1</strong>。  

2. 將 NT AUTHORITY\Authenticated Users 新增到 [WinRMRemoteWMIUsers__] 群組。  

#### <a name="create-files-and-folders-on-file1"></a>在 FILE1 上建立檔案和資料夾  

1.  在 FILE1 上建立新的 NTFS 磁碟區，然後建立下列資料夾：D:\Finance Documents。  

2.  建立指定下列詳細資料的檔案：  

    -   **Finance Memo.docx**：在文件中新增一些財務相關文字。 例如，「關於誰可以存取財務檔的商務規則已變更。 財務文件現在只能由 FinanceExpert 群組的成員存取。 沒有其他部門或群組有存取權。」 在環境中實作之前必須評估此變更的影響。 請確定這份文件的每個頁面的頁尾上都有「Contoso 公司機密」的字樣。  

    -   **要求核准 Hire.docx**：在文件中建立一個收集應徵者資訊的表單。 文件中必須包含下列欄位： **應徵者姓名、身分證號碼、職稱、建議薪資、開始日期、主管姓名、部門**。 在包含**主管簽章、核准薪資、職務確認**和**職務狀態**表單的文件中新增其他區段。   
        啟用文件權限管理。  

    -   **Word Document1.docx**：將一些測試內容新增到這份文件。  

    -   **Word Document2.docx**：將測試內容新增到這份文件。  

    -   **Workbook1 .xlsx**  

    -   **Workbook2 .xlsx**  

    -   在桌面上建立稱為「規則運算式」的資料夾。 在名為 **RegEx-SSN** 的資料夾下建立文字文件。 在檔案中輸入下列內容，然後儲存並關閉檔案：   
        ^(?!000)([0-7]\d{4}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$  

3.  將 D:\Finance Documents 資料夾共用為財務文件，並允許每個人都對該共用擁有讀取和寫入存取權。  

> [!NOTE]  
> 系統或開機磁碟區 C: 上預設不會啟用集中存取原則。  

#### <a name="install-active-directory-rights-management-services"></a><a name="BKMK_CS1"></a>安裝 Active Directory Rights Management Services  
透過伺服器管理員新增 Active Directory Rights Management Services (AD RMS) 和所有必要的功能。 選擇所有預設值。  

###### <a name="to-install-active-directory-rights-management-services"></a>安裝 Active Directory Rights Management Services  

1. 以 CONTOSO\Administrator 的身分或 Domain Admins 群組的成員身分登入 FILE1。  

   > [!IMPORTANT]  
   > 為了安裝 AD RMS 伺服器角色，安裝程式帳戶 (在此例中為 CONTOSO\Administrator) 必須同時具有 AD RMS 伺服器電腦上本機 Administrators 群組以及 Active Directory 中 Enterprise Admins 群組的成員資格。  

2. 在 [伺服器管理員] 中，按一下 [新增角色及功能]。 [新增角色及功能精靈] 隨即顯示。  

3. 在 [在您開始前] 畫面上，按 [下一步]。  

4. 在 [選取安裝類型] 畫面上，按一下 [角色/功能型安裝]，然後按一下 [下一步]。  

5. 在 [選取伺服器目標] 畫面上，按一下 [下一步]。  

6. 在 [選取伺服器角色] 畫面上，選取 [Active Directory Rights Management Services] 旁邊的方塊，然後按一下 [下一步]。  

7. 在 [新增 Active Directory Rights Management Services 所需的功能嗎?] 對話方塊中，按一下 [新增功能]。  

8. 在 [選取伺服器角色] 畫面上，按一下 [下一步]。  

9. 在 [選取要安裝的功能] 畫面上，按一下 [下一步]。  

10. 在 [Active Directory Rights Management Services] 畫面上，按一下 [下一步]。  

11. 在 [選取角色服務] 畫面上，按一下 [下一步]。  

12. 在 [網頁伺服器 (IIS)] 畫面上，按 [下一步]。  

13. 在 [選取角色服務] 畫面上，按一下 [下一步]。  

14. 在 [確認安裝選項] 畫面上，按一下 [安裝]。  

15. 安裝完成後，在 [安裝進度]畫面上，按一下 [執行其他設定]。 [AD RMS 設定精靈] 隨即開啟。  

16. 在 [AD RMS] 畫面上，按一下 [下一步]。  

17. 在 [AD RMS 叢集] 畫面上，選取 [建立新的 AD RMS 根叢集]，然後按一下 [下一步]。  

18. 在 [設定資料庫] 畫面上，按一下 [在此伺服器上使用 Windows 內部資料庫]，然後按一下 [下一步]。  

    > [!NOTE]  
    > 測試環境建議使用 Windows 內部資料庫，只是因為它在 AD RMS 叢集中不支援多個伺服器。 生產部署應該使用分開的資料庫伺服器。  

19. 在 [**服務帳戶**] 畫面的 [**網域使用者帳戶**] 中，按一下 [**指定**]，然後指定使用者名稱（**contoso\rms**）和密碼（<strong>pass@word1</strong>），再按一下 **[確定]** ，然後按 **[下一步**]。  

20. 在 [密碼編譯模式] 畫面上，按一下 [密碼編譯模式 2]。  

21. 在 [叢集金鑰儲存] 畫面上，按一下 [下一步]。  

22. 在 [叢集**金鑰密碼**] 畫面的 [**密碼**] 和 [**確認密碼**] 方塊中，輸入<strong>pass@word1</strong>，然後按 **[下一步]** 。  

23. 在 [叢集網站] 畫面上，請確定 [預設的網站] 已選取，然後按一下 [下一步]。  

24. 在 [叢集位址] 畫面上，選取 [使用未加密連線] 選項，在 [完整網域名稱] 方塊中，輸入 **FILE1.contoso.com**，然後按一下 [下一步]。  

25. 在 [授權人憑證名稱] 畫面上，接受文字方塊中的預設名稱 (**FILE1**)，按一下 [下一步]。  

26. 在 [SCP 登錄] 畫面上，選取 [立即登錄 SCP]，然後按一下 [下一步]。  

27. 在 [確認] 畫面上，按一下 [安裝]。  

28. 在 [結果] 畫面上，按一下 [關閉]，然後按一下 [安裝進度] 畫面上的 [關閉]。 完成時，請使用提供的密碼（<strong>pass@word1</strong>）登出並登入為 contoso\rms。  

29. 啟動 AD RMS 主控台，並瀏覽至 [權限原則範本]。  

    若要開啟 AD RMS 主控台，在 [伺服器管理員] 中，按一下主控台樹狀目錄的 [本機伺服器]，按一下 [工具]，然後按一下 [Active Directory Rights Management Services]。  

30. 按一下位於右窗格中的 [建立散佈的權限原則] 範本，按一下 [新增]，並選取下列資訊：  

    -   語言：美式英文  

    -   名稱：只限 Contoso 財務管理人員  

    -   描述：只限 Contoso 財務管理人員  

    按一下 [新增]，再按 [下一步]。  

31. 在 [使用者和許可權] 區段下，按一下 [**使用者和許可權**]，按一下 [**新增**]，輸入<strong>financeadmin@contoso.com</strong>，然後按一下 **[確定]** 。  

32. 選取 [完全控制]，保留 [授與擁有者 (作者) 完全控制權限永不過期] 的選取狀態。  

33. 不變更其他設定，按一下剩餘的索引標籤，然後按一下 [完成]。 以 CONTOSO\Administrator 身分登入。  

34. 流覽至 [C:\inetpub\wwwroot\\_wmcs] 資料夾，選取 [ServerCertification] 檔案，然後將 [已驗證的使用者] 加入檔案的 [讀取] 和 [寫入] 許可權。  

35. 開啟 Windows PowerShell 並執行 `Get-FsrmRmsTemplate`。 確認在此程序中使用此命令可以看到您在前面步驟中建立的 RMS 範本。  

> [!IMPORTANT]  
> 如果您要檔案伺服器立即變更以便測試，則需要執行下列動作：  
>   
> 1.  在檔案伺服器 FILE1 上，開啟提升權限的命令提示字元，然後執行下列命令：  
>   
>     -   gpupdate /force。  
>     -   NLTEST /SC_RESET:contoso.com  
> 2.  在網域控制站 (DC1) 上，複寫 Active Directory。  
>   
>     如需如何強制執行 Active Directory 複寫的詳細資訊，請參閱 [Active Directory 複寫](https://technet.microsoft.com/library/cc794809(WS.10).aspx)。  

(選擇性) 不要使用 [伺服器管理員] 中的 [新增角色和功能精靈]，而是改用 Windows PowerShell 來安裝和設定 AD RMS 伺服器角色，如下列程序中所示。  

###### <a name="to-install-and-configure-an-ad-rms-cluster-in-windows-server-2012-using-windows-powershell"></a>在 Windows Server 2012 中使用 Windows PowerShell 安裝和設定 AD RMS 叢集  

1. 以 CONTOSO\Administrator 的身分登入，密碼為： <strong>pass@word1</strong>。  

   > [!IMPORTANT]  
   > 為了安裝 AD RMS 伺服器角色，安裝程式帳戶 (在此例中為 CONTOSO\Administrator) 必須同時具有 AD RMS 伺服器電腦上本機 Administrators 群組以及 Active Directory 中 Enterprise Admins 群組的成員資格。  

2. 在伺服器桌面上，以滑鼠右鍵按一下工作列上的 [Windows PowerShell] 圖示並選取 [以系統管理員身分執行] ，以系統管理員權限開啟 Windows PowerShell 命令提示字元。  

3. 若要使用伺服器管理員 Cmdlet 來安裝 AD RMS 伺服器角色，請輸入：  

   ```  
   Add-WindowsFeature ADRMS '"IncludeAllSubFeature '"IncludeManagementTools  
   ```  

4. 建立 Windows PowerShell 磁碟機來代表您要安裝的 AD RMS 伺服器。  

   例如，若要建立名為 RC 的 Windows PowerShell 磁碟機，以安裝和設定 AD RMS 根叢集中的第一部伺服器，請輸入：  

   ```  
   Import-Module ADRMS  
   New-PSDrive -PSProvider ADRMSInstall -Name RC -Root RootCluster  
   ```  

5. 設定表示必要組態設定的磁碟機命名空間中物件的內容。  

   例如，若要設定 AD RMS 服務帳戶，請在 Windows PowerShell 命令提示字元中輸入：  

   ```  
   $svcacct = Get-Credential  
   ```  

   當 [Windows 安全性] 對話方塊出現時，輸入 AD RMS 服務帳戶網域使用者名稱 CONTOSO\RMS 及指派的密碼。  

   接著，將 AD RMS 服務帳戶指派給 AD RMS 叢集設定，輸入下列命令：  

   ```  
   Set-ItemProperty -Path RC:\ -Name ServiceAccount -Value $svcacct  
   ```  

   接著，設定 AD RMS 伺服器使用 Windows 內部資料庫，在 Windows PowerShell 命令提示字元中輸入：  

   ```  
   Set-ItemProperty -Path RC:\ClusterDatabase -Name UseWindowsInternalDatabase -Value $true  
   ```  

   接著，為了安全地將叢集金鑰密碼儲存在變數中，在 Windows PowerShell 命令提示字元中輸入：  

   ```  
   $password = Read-Host -AsSecureString -Prompt "Password:"  
   ```  

   輸入叢集金鑰密碼，然後按下 ENTER。  

   接著，將密碼指派給 AD RMS 安裝，在 Windows PowerShell 命令提示字元中輸入：  

   ```  
   Set-ItemProperty -Path RC:\ClusterKey -Name CentrallyManagedPassword -Value $password  
   ```  

   接著，設定 AD RMS 叢集位址，在 Windows PowerShell 命令提示字元中輸入：  

   ```  
   Set-ItemProperty -Path RC:\ -Name ClusterURL -Value "http://file1.contoso.com:80"  
   ```  

   接著，為 AD RMS 安裝指派 SLC 名稱，在 Windows PowerShell 命令提示字元中輸入：  

   ```  
   Set-ItemProperty -Path RC:\ -Name SLCName -Value "FILE1"  
   ```  

   接著，設定 AD RMS 叢集的服務連線點 (SCP)，在 Windows PowerShell 命令提示字元中輸入：  

   ```  
   Set-ItemProperty -Path RC:\ -Name RegisterSCP -Value $true  
   ```  

6. 執行 **Install-ADRMS** Cmdlet。 這個 Cmdlet 除了安裝 AD RMS 伺服器角色及設定伺服器之外，也會安裝 AD RMS 所需的其他功能 (如有必要)。  

   例如，若要變更名為 RC 的 Windows PowerShell 磁碟機並安裝和設定 AD RMS，請輸入：  

   ```  
   Set-Location RC:\  
   Install-ADRMS -Path.  
   ```  

   當 Cmdlet 提示您確認是否要開始安裝時，輸入 "Y" 。  

7. 以 CONTOSO\Administrator 的身分登入，並使用提供的密碼（"pass@word1"）登入為 CONTOSO\RMS。  

   > [!IMPORTANT]  
   > 為了管理 AD RMS 伺服器，您用來登入並管理伺服器 (在此例中為 CONTOSO\RMS) 的帳戶必須同時具有 AD RMS 伺服器電腦上本機 Administrators 群組以及 Active Directory 中 Enterprise Admins 群組的成員資格。  

8. 在伺服器桌面上，以滑鼠右鍵按一下工作列上的 [Windows PowerShell] 圖示並選取 [以系統管理員身分執行] ，以系統管理員權限開啟 Windows PowerShell 命令提示字元。  

9. 建立 Windows PowerShell 磁碟機來代表您要設定的 AD RMS 伺服器。  

    例如，若要建立名為 RC 的 Windows PowerShell 磁碟機以設定 AD RMS 根叢集，請輸入：  

    ```  
    Import-Module ADRMSAdmin `  
    New-PSDrive -PSProvider ADRMSAdmin -Name RC -Root http://localhost -Force -Scope Global  
    ```  

10. 若要在 AD RMS 安裝中為 Contoso 財務系統管理員建立新的權限範本，並指派具有完全控制的使用者權限，請在 Windows PowerShell 命令提示字元中輸入：  

    ```  
    New-Item -Path RC:\RightsPolicyTemplate '"LocaleName en-us -DisplayName "Contoso Finance Admin Only" -Description "Contoso Finance Admin Only" -UserGroup financeadmin@contoso.com  -Right ('FullControl')  
    ```  

11. 若要確認可以看到 Contoso 財務系統管理員的新的權限範本，請在 Windows PowerShell 命令提示字元中：  

    ```  
    Get-FsrmRmsTemplate  
    ```  

    檢閱這個 Cmdlet 的輸出，確認您在上一個步驟中建立的 RMS 範本存在。  

### <a name="build-the-mail-server-srv1"></a>建立郵件伺服器 (SRV1)  
SRV1 是 SMTP/POP3 郵件伺服器。 您需要設定郵件伺服器，以便在「拒絕存取」協助案例中傳送電子郵件通知。  

在這台電腦上設定 Microsoft Exchange Server。 如需詳細資訊，請參閱 [如何安裝 Exchange Server](https://go.microsoft.com/fwlink/?prd=12364)。  

### <a name="build-the-client-virtual-machine-client1"></a>建立用戶端虛擬機器 (CLIENT1)  

##### <a name="to-build-the-client-virtual-machine"></a>建立用戶端虛擬機器  

1. 將 CLIENT1 連線到 ID_AD_Network。  

2. 安裝 Microsoft Office 2010。  

3. 以 Contoso\Administrator 的身分登入，並使用下列資訊來設定 Microsoft Outlook。  

   - 您的名稱：檔案系統管理員  

   - 電子郵件地址： fileadmin@contoso.com  

   - 帳戶類型：POP3  

   - 內送郵件伺服器：SRV1 的靜態 IP 位址  

   - 外寄郵件伺服器：SRV1 的靜態 IP 位址  

   - 使用者名稱： fileadmin@contoso.com  

   - 記住密碼：選取  

4. 在 contoso\administrator 桌面上建立 Outlook 的捷徑。  

5. 開啟 Outlook 並處理所有「第一次啟動」的訊息。  

6. 刪除任何產生的測試訊息。  

7. 針對用戶端虛擬機器上指向 \\\FILE1\Finance 檔的所有使用者，在桌面上建立新的短剪。  

8. 需要重新開機。  

##### <a name="enable-access-denied-assistance-on-the-client-virtual-machine"></a>啟用用戶端虛擬機器上的拒絕存取協助  

1.  開啟 [登錄編輯程式]，並瀏覽至 **HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Explorer**。  

    -   將 **EnableShellExecuteFileStreamCheck** 設定為 **1**。  

    -   值：DWORD  

## <a name="lab-setup-for-deploying-claims-across-forests-scenario"></a><a name="BKMK_CF"></a>實驗室設定，用於跨樹系部署宣告案例  

### <a name="build-a-virtual-machine-for-dc2"></a><a name="BKMK_2.1"></a>建立 DC2 的虛擬機器  

-   從 Windows Server 2012 ISO 建立虛擬機器。  

-   建立名稱為 DC2 的虛擬機器。  

-   將虛擬機器連線到 ID_AD_Network。  

> [!IMPORTANT]  
> 將虛擬機器加入網域與跨樹系部署宣告類型，需要虛擬機器能夠解析相關網域的 FQDN。 您可能必須手動設定虛擬機器上的 DNS 設定，以完成這項作業。 如需詳細資訊，請參閱 [設定虛擬機器](https://technet.microsoft.com/library/cc732470%28v=ws.10%29.aspx)。  
>   
> 所有虛擬機器映像 (伺服器與用戶端) 必須重新設定為使用靜態 IP 第 4 版 (IPv4) 位址及網域名稱系統 (DNS) 用戶端設定。 如需詳細資訊，請參閱 [設定 DNS 用戶端的靜態 IP 位址](https://go.microsoft.com/fwlink/?LinkId=150952)。  

### <a name="set-up-a-new-forest-called-adatumcom"></a><a name="BKMK_2.2"></a>設定名為 adatum.com 的新樹系  

##### <a name="to-install-active-directory-domain-services"></a>安裝 Active Directory 網域服務  

1. 將虛擬機器連線到 ID_AD_Network。 以系統管理員身分登入 DC2，並使用密碼<strong>Pass@word1</strong>。  

2. 在 [伺服器管理員] 中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。  

3. 在 [在您開始前] 頁面中按 [下一步]。  

4. 在 [選取安裝類型] 頁面上，按一下 [角色型或功能型安裝]，然後按一下 [下一步]。  

5. 在 [選取目的地伺服器] 頁面上，依序按一下 [從伺服器集區選取伺服器] 和您要安裝 Active Directory 網域服務 (AD DS) 的伺服器名稱，再按一下 [下一步]。  

6. 在 [選取伺服器角色] 頁面上，選取 [Active Directory 網域服務]。 在 [新增角色及功能精靈] 對話方塊中，按一下 [新增功能]，然後按 [下一步]。  

7. 在 [選取功能] 頁面上，按一下 [下一步]。  

8. 檢閱 [AD DS] 頁面上的資訊，然後按一下 [下一步]。  

9. 在 [確認] 頁面上，按一下 [安裝]。 [結果] 頁面上的功能安裝進度列表示正在安裝角色。  

10. 在 [結果] 頁面上，確認安裝成功，然後按一下螢幕右上角 [管理] 旁邊具有驚嘆號的警告圖示 。 在 [工作] 清單中，按一下 [將此伺服器升級為網域控制站] 連結。  

    > [!IMPORTANT]  
    > 如果您在此時關閉安裝精靈，而不是按一下 [將此伺服器升級為網域控制站]，則可以按一下 [伺服器管理員] 中的 [工作]，繼續 AD DS 安裝。  

11. 在 [部署設定] 頁面上，按一下 [新增新的樹系]，輸入根網域的名稱 **adatum.com**，然後按一下 [下一步]。  

12. 在 [**網域控制站選項**] 頁面上，選取網域和樹系功能等級為 Windows Server 2012， <strong>pass@word1</strong>指定 DSRM 密碼，然後按 **[下一步]** 。  

13. 在 [DNS 選項] 頁面上，按一下 [下一步]。  

14. 在 [其他選項] 頁面上，按一下 [下一步]。  

15. 在 [路徑] 頁面上，輸入 Active Directory 資料庫、記錄檔以及 SYSVOL 資料夾的位置 (或接受預設位置)，然後按一下 [下一步]。  

16. 在 [檢閱選項] 頁面上，確認您的選項，然後按一下 [下一步]。  

17. 在 [先決條件檢查] 頁面上，確認先決條件驗證已完成，然後按一下 [安裝]。  

18. 在 [結果] 頁面上，確認伺服器已成功設定為網域控制站，然後按一下 [關閉]。  

19. 重新啟動伺服器，以完成 AD DS 安裝。 (預設會自動重新啟動。)  

> [!IMPORTANT]  
> 設定好這兩個樹系之後，若要確定網路已正確設定，您必須執行下列動作：  
>   
> -   以 adatum\administrator 身分登入 adatum.com。 開啟命令提示字元視窗，輸入 **nslookup contoso.com**，然後按下 ENTER。  
> -   以 contoso\administrator 身分登入 contoso.com。 開啟命令提示字元視窗，輸入 **nslookup adatum.com**，然後按下 ENTER。  
>   
> 如果執行這些命令沒有錯誤，表示樹系可彼此通訊。 如需 nslookup 錯誤的詳細資訊，請參閱主題 [使用 NSlookup.exe](https://support.microsoft.com/kb/200525)中的＜疑難排解＞一節  

### <a name="set-contosocom-as-a-trusting-forest-to-adatumcom"></a><a name="BKMK_2.22"></a>將 contoso.com 設定為 adatum.com 的信任樹系  
在這個步驟中，您將會在 Adatum Corporation 站台和 Contoso, Ltd. 站台之間建立信任關係。  

##### <a name="to-set-contoso-as-a-trusting-forest-to-adatum"></a>將 Contoso 設定為 Adatum 信任的樹系  

1.  以系統管理員身分登入 DC2。 在 [開始] 畫面上，輸入 domain.msc。  

2.  在主控台樹狀目錄中，以滑鼠右鍵按一下 adatum.com，然後按一下 [內容]。  

3.  在 [信任] 索引標籤上，按一下 [新增信任]，然後按一下 [下一步]。  

4.  在 [信任名稱] 頁面上，在 [網域名稱系統 (DNS) 名稱] 欄位中輸入 **contoso.com**，然後按一下 [下一步]。  

5.  在 [信任類型] 頁面上，按一下 [樹系信任]，然後按一下 [下一步]。  

6.  在 [信任方向] 頁面上，按一下 [雙向]。  

7.  在 [信任方] 頁面上，按一下 [這個網域和指定網域兩者]，然後按一下 [下一步]。  

8.  依照精靈中的指示繼續執行。  

### <a name="create-additional-users-in-the-adatum-forest"></a><a name="BKMK_2.4"></a>在 Adatum 樹系中建立其他使用者  
使用<strong>pass@word1</strong>的密碼建立 Jeff Low 使用者，並使用值**Adatum**指派公司屬性。  

##### <a name="to-create-a-user-with-the-company-attribute"></a>建立包含公司屬性的使用者  

1.  在 Windows PowerShell 中開啟提升權限的命令提示字元，貼上下列程式碼：  

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

### <a name="create-the-company-claim-type-on-adataumcom"></a><a name="BKMK_2.5"></a>在 adataum.com 上建立公司宣告類型  

##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>使用 Windows PowerShell 建立宣告類型  

1.  以系統管理員身分登入 adatum.com。  

2.  在 Windows PowerShell 中開啟提升權限的命令提示字元，輸入下列程式碼：  

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

### <a name="enable-the-company-resource-property-on-contosocom"></a><a name="BKMK_2.55"></a>在 contoso.com 上啟用 [公司資源] 屬性  

##### <a name="to-enable-the-company-resource-property-on-contosocom"></a>啟用 contoso.com 的公司資源內容  

1.  以系統管理員身分登入 contoso.com。  

2.  在 [伺服器管理員] 中，按一下 [工具]，然後按一下 [Active Directory 管理中心]。  

3.  在 [Active Directory 管理中心] 的左窗格中，按一下 [樹狀檢視]。 在左窗格中，按一下 [動態存取控制]，再按兩下 [資源內容]。  

4.  從 [資源內容] 清單中選取 [公司]，以滑鼠右鍵按一下並選取 [內容]。 在 [建議值] 區段中，按一下 [新增] 以新增建議的值：Contoso 及 Adatum，然後按一下 [確定] 兩次。  

5.  從 [資源內容] 清單中選取 [公司]，以滑鼠右鍵按一下並選取 [啟用]。  

### <a name="enable-dynamic-access-control-on-adatumcom"></a><a name="BKMK_2.6"></a>在 adatum.com 上啟用動態存取控制  

##### <a name="to-enable-dynamic-access-control-for-adatumcom"></a>啟用 adatum.com 的動態存取控制  

1.  以系統管理員身分登入 adatum.com。  

2.  開啟 [群組原則管理主控台]，按一下 [adatum.com]，再按兩下 [網域控制站]。  

3.  以滑鼠右鍵按一下 [預設網域控制站原則]，然後選取 [編輯]。  

4.  在 [群組原則管理編輯器] 視窗中，依序按兩下 [電腦設定]、[原則]、[系統管理範本]、[系統]、[KDC]。  

5.  按兩下 [宣告、複合驗證和 Kerberos 防護的 KDC 支援] 並選取 [啟用] 旁邊的選項。 您必須啟用這個設定，才能使用集中存取原則。  

6.  開啟提升權限的命令提示字元，然後執行下列命令：  

    ```  
    gpupdate /force  
    ```  

### <a name="create-the-company-claim-type-on-contosocom"></a><a name="BKMK_2.8"></a>在 contoso.com 上建立公司宣告類型  

##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>使用 Windows PowerShell 建立宣告類型  

1.  以系統管理員身分登入 contoso.com。  

2.  在 Windows PowerShell 中開啟提升權限的命令提示字元，然後輸入下列程式碼：  

    ```  
    New-ADClaimType '"SourceTransformPolicy `  
    '"DisplayName 'Company' `  
    '"ID 'ad://ext/Company:ContosoAdatum' `  
    '"IsSingleValued $true `  
    '"ValueType 'string' `  

    ```  

### <a name="create-the-central-access-rule"></a><a name="BKMK_2.9"></a>建立集中存取規則  

##### <a name="to-create-a-central-access-rule"></a>建立集中存取規則  

1. 在 [Active Directory 管理中心] 的左窗格中，按一下 [樹狀檢視]。 在左窗格中，按一下 [動態存取控制]，然後按一下 [集中存取規則]。  

2. 以滑鼠右鍵按一下 [集中存取規則]，按一下 [新增]，再按一下 [集中存取規則]。  

3. 在 [名稱] 欄位中輸入 **AdatumEmployeeAccessRule**。  

4. 在 [權限] 區段中，選取 [使用下列權限做為目前的權限] 選項，按一下 [編輯]，然後按一下 [新增]。 按一下 [選取主體] 連結，輸入**已驗證的使用者**，然後按一下 [確定]。  

5. 在 [權限的權限項目] 對話方塊中，按一下 [新增條件]，並輸入下列條件：[**User**] [**Company**] [**Equals**] [**Value**] [**Adatum**]。 權限應該是 [修改]、[讀取和執行]、[讀取]、[寫入]。  

6. 按一下 [確定]。  

7. 按一下 [確定] 三次，以完成並返回 Active Directory 管理中心。  

   ![解決方案引導](media/Appendix-B--Setting-Up-the-Test-Environment/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等命令</em>***  

   下列 Windows PowerShell 指令程式會執行與前述程序相同的功能。 請逐行各輸入一個指令程式，儘管有些指令程式可能因為受制於內文格式而自動換行拆成好幾行。  

   ```  
   New-ADCentralAccessRule `  
   -CurrentAcl:"O:SYG:SYD:AR(A;;FA;;;OW)(A;;FA;;;BA)(A;;FA;;;SY)(XA;;0x1301bf;;;AU;(@USER.ad://ext/Company:ContosoAdatum == `"Adatum`"))" `  
   -Name:"AdatumEmployeeAccessRule" `  
   -ProposedAcl:$null `  
   -ProtectedFromAccidentalDeletion:$true `  
   -Server:"contoso.com" `  
   ```  

### <a name="create-the-central-access-policy"></a><a name="BKMK_2.10"></a>建立集中存取原則  

##### <a name="to-create-a-central-access-policy"></a>建立集中存取原則  

1.  以系統管理員身分登入 contoso.com。  

2.  在 Windows PowerShell 中開啟提升權限的命令提示字元，然後貼上下列程式碼：  

    ```  
    New-ADCentralAccessPolicy "Adatum Only Access Policy"   
    Add-ADCentralAccessPolicyMember "Adatum Only Access Policy" `  
    -Member "AdatumEmployeeAccessRule" `  
    ```  

### <a name="publish-the-new-policy-through-group-policy"></a><a name="BKMK_2.11"></a>透過群組原則發佈新原則  

##### <a name="to-apply-the-central-access-policy-across-file-servers-through-group-policy"></a>透過群組原則在檔案伺服器之間套用集中存取原則  

1.  在 [開始] 畫面上輸入**系統管理工具**，然後按一下 [搜尋] 列中的 [設定]。 按一下 [設定] 結果中的 [系統管理工具]。 從 [系統管理工具] 資料夾開啟群組原則管理主控台。  

    > [!TIP]  
    > 如果停用 [顯示系統管理工具] 設定，[系統管理工具] 資料夾及其內容就不會出現在 [設定] 結果中。  

2.  以滑鼠右鍵按一下 contoso.com 網域，按一下 [**在這個網域中建立 GPO 並連結到**]  

3.  輸入 GPO 的描述性名稱，例如 **AdatumAccessGPO**，然後按一下 [確定]。  

##### <a name="to-apply-the-central-access-policy-to-the-file-server-through-group-policy"></a>透過群組原則將集中存取原則套用到檔案伺服器  

1.  在 [開始] 畫面上，在 [搜尋] 方塊中輸入**群組原則管理**。 從 [系統管理工具] 資料夾開啟 [群組原則管理]。  

    > [!TIP]  
    > 如果停用 [顯示系統管理工具] 設定，[系統管理工具] 資料夾及其內容就不會出現在 [設定] 結果中。  

2.  如下所示瀏覽並選取 Contoso：Group Policy Management\Forest: contoso.com\Domains\contoso.com。  

3.  以滑鼠右鍵按一下 [AdatumAccessGPO]原則，然後選取 [編輯]。  

4.  在 [群組原則管理編輯器] 中，按一下 [電腦設定]，展開 [原則]，展開 [Windows 設定]，然後按一下 [安全性設定]。  

5.  展開 [檔案系統]，以滑鼠右鍵按一下 [集中存取原則]，然後按一下 [管理集中存取原則]。  

6.  在 [集中存取原則設定] 對話方塊中，按一下 [新增]，選取 [僅限 Adatum 存取原則]，然後按一下 [確定]。  

7.  關閉 [群組原則管理編輯器]。 您現在已將集中存取原則新增到群組原則。  

### <a name="create-the-earnings-folder-on-the-file-server"></a><a name="BKMK_2.12"></a>在檔案伺服器上建立收益資料夾  
在 FILE1 上建立新的 NTFS 磁碟區，然後建立下列資料夾：D:\Earnings。  

> [!NOTE]  
> 系統或開機磁碟區 C: 上預設不會啟用集中存取原則。  

### <a name="set-classification-and-apply-the-central-access-policy-on-the-earnings-folder"></a><a name="BKMK_2.13"></a>設定分類並套用 [收益] 資料夾上的集中存取原則  

##### <a name="to-assign-the-central-access-policy-on-the-file-server"></a>指派檔案伺服器上的集中存取原則  

1. 在 [HYPER-V 管理員] 中，連線至伺服器 FILE1。 使用 Contoso\Administrator 和密碼<strong>pass@word1</strong>來登入伺服器。  

2. 開啟提升權限的命令提示字元，輸入：**gpupdate /force** 這可確保您的群組原則變更在伺服器上生效。  

3. 您也需要重新整理 Active Directory 的全域資源內容。 開啟 Windows PowerShell，輸入： `Update-FSRMClassificationpropertyDefinition`，然後按 ENTER 鍵。 關閉 Windows PowerShell。  

4. 開啟 [Windows 檔案總管]，然後瀏覽至 D:\EARNINGS。 以滑鼠右鍵按一下 [Earnings] 資料夾，再按一下 [內容]。  

5. 按一下 [**分類**] 索引標籤。選取 [**公司**]，然後在 [**值**] 欄位中選取 [ **Adatum** ]。  

6. 按一下 [變更]，從下拉式功能表選取 [僅限 Adatum 存取原則]，然後按一下 [套用]。  

7. 按一下 [**安全性**] 索引標籤，然後按一下 [ **Advanced**]，再按一下 [**集中原則**] 索引標籤。您應該會看到列出的**AdatumEmployeeAccessRule** 。 您可以展開項目以檢視在 Active Directory 中建立規則時設定的所有權限。  

8. 按一下 [確定] 返回 [Windows 檔案總管]。  




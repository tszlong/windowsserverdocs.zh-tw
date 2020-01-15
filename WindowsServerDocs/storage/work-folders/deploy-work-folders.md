---
ms.assetid: d2429185-9720-4a04-ad94-e89a9350cdba
title: 部署工作資料夾
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dongill
ms.author: jgerend
ms.date: 6/24/2017
description: 如何部署工作資料夾，包括安裝伺服器角色、建立同步共用和建立 DNS 記錄。
ms.openlocfilehash: b8ed2578f3f55b6540315b8744bcb72523869044
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950218"
---
# <a name="deploying-work-folders"></a>部署工作資料夾

>適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows 10、Windows 8.1、Windows 7

本主題討論部署工作資料夾所需的步驟。 我們假設您已經閱讀過[規劃工作資料夾部署](plan-work-folders.md)。  
  
 若要部署工作資料夾，過程中可能會涉及多個伺服器和多項技術，請使用下列步驟來完成。  
  
> [!TIP]
>  最簡單的工作資料夾部署是單一檔案伺服器 (通常稱為同步伺服器)，但不支援透過網際網路同步，這對於測試實驗室是實用的部署，或者可以做為加入網域之用戶端電腦的同步解決方案。 若要建立簡單部署，至少需要遵循下列幾個步驟： 
>  -   步驟 1：取得 SSL 憑證  
>  -   步驟 2：建立 DNS 記錄 
>  -   步驟 3：在檔案伺服器上安裝工作資料夾  
>  -   步驟 4：在同步伺服器上繫結 SSL 憑證
>  -   步驟 5：建立工作資料夾的安全性群組  
>  -   步驟 7：建立使用者資料的同步共用  
  
## <a name="step-1-obtain-ssl-certificates"></a>步驟 1：取得 SSL 憑證  
 工作資料夾會使用 HTTPS 安全地在工作資料夾用戶端與工作資料夾伺服器之間同步處理檔案。 工作資料夾使用的 SSL 憑證需求如下：  
  
- 憑證必須由受信任的憑證授權單位發出。 對於大部分的工作資料夾實作，建議使用公用信任的 CA，因為憑證會由未加入網域、以網際網路為基礎的裝置使用。  
  
- 憑證必須是有效的。  
  
- 憑證的私密金鑰必須可以匯出 (因為您需要在多個伺服器上安裝憑證)。  
  
- 憑證的主體名稱必須包含公用工作資料夾 URL，用來在網際網路上探索工作資料夾服務；它的格式必須是 `workfolders.` *<domain_name>* 。  
  
- 主體別名 (SAN) 必須顯示在憑證上，列出每個使用中的同步伺服器的伺服器名稱。

  工作資料夾憑證管理[部落格](https://blogs.technet.microsoft.com/filecab/2013/08/09/work-folders-certificate-management/)提供有關使用憑證和工作資料夾的資訊。
  
## <a name="step-2-create-dns-records"></a>步驟 2：建立 DNS 記錄  
 若要允許使用者透過網際網路進行同步，您必須在公用 DNS 中建立主機 (A) 記錄，以便允許網際網路用戶端解析工作資料夾 URL。 這個 DNS 記錄應該解析至反向 Proxy 伺服器的外部介面。  
  
 在您的內部網路上，於 DNS 命名的工作資料夾中建立 CNAME 記錄，其解析到工作資料夾伺服器的 FDQN。 當工作資料夾用戶端使用自動探索時，用來探索工作資料夾伺服器的 URL 會是 HTTPs：\//workfolders.domain.com。 如果您打算使用自動探索，工作資料夾 CNAME 記錄必須存在於 DNS 中。  
  
## <a name="step-3-install-work-folders-on-file-servers"></a>步驟 3：在檔案伺服器上安裝工作資料夾  
 您可以使用伺服器管理員或 Windows PowerShell，在本機或透過網路從遠端，於加入網域的伺服器上安裝工作資料夾。 如果您要在網路上設定多個同步伺服器，這個方法很有用。  
  
若要在伺服器管理員中部署角色，請執行下列動作：  
  
1.  啟動 **\[新增角色及功能精靈\]** 。  
  
2.  在 **\[選取安裝類型\]** 頁面上，選擇 **\[角色型或功能型部署\]** 。  
  
3.  在 **\[選取目的地伺服器\]** 頁面上，選取您想要在上面安裝工作資料夾的伺服器。  
  
4.  在 **\[選取伺服器角色\]** 頁面上，依序展開 **\[檔案和存放服務\]** 、 **\[檔案和 iSCSI 服務\]** ，然後選取 **\[工作資料夾\]** 。  
  
5.  系統詢問您是否要安裝 **\[可裝載 IIS 的 Web 核心\]** 時，按一下 **\[確定\]** ，即可安裝工作資料夾所需的網際網路資訊服務 (IIS) 的最低版本。  
  
6.  按 **\[下一步\]** ，直到完成精靈為止。

若要使用 Windows PowerShell 來部署角色，請使用下列 Cmdlet：
  
```powershell  
Add-WindowsFeature FS-SyncShareService  
```

## <a name="step-4-binding-the-ssl-certificate-on-the-sync-servers"></a>步驟 4：在同步伺服器上繫結 SSL 憑證
 工作資料夾會安裝可裝載 IIS 的 Web 核心，它是 IIS 元件，是專為不需要完整安裝 IIS 的情況下啟用 Web 服務所設計的。 安裝可裝載 IIS 的 Web 核心之後，應該將伺服器的 SSL 憑證繫結至檔案伺服器上的預設網站。 不過，可裝載 IIS 的 Web 核心不會安裝 IIS 管理主控台。

 將憑證繫結至預設網站介面有兩個選項。 若要使用任一選項，您必須將憑證的私密金鑰安裝到電腦的個人存放區。

- 在已經安裝的伺服器上利用 IIS 管理主控台。 從主控台內，連線到您想要管理的檔案伺服器，然後為該伺服器選取預設網站。 預設網站會顯示已停用，但是您仍然可以編輯網站的繫結，並選取憑證以繫結至該網站。

- 使用 netsh 命令，將憑證繫結至預設網站 https 介面。 命令如下所示：

    ```
    netsh http add sslcert ipport=<IP address>:443 certhash=<Cert thumbprint> appid={CE66697B-3AA0-49D1-BDBD-A25C8359FD5D} certstorename=MY
    ```

## <a name="step-5-create-security-groups-for-work-folders"></a>步驟 5：建立工作資料夾的安全性群組
 建立同步共用之前，Domain Admins 或 Enterprise Admins 群組的成員需要在 Active Directory Domain Services (AD DS) 中為工作資料夾建立一些安全性群組 (他們也需要如步驟 6 中所述委派一些控制權)。 您所需的群組如下：

- 一個各個同步共用的群組，指定哪些使用者允許與同步共用進行同步

- 一個所有工作資料夾系統管理員所屬的群組，以便他們在每個連結使用者至正確同步伺服器 (如果要使用多個同步伺服器的話) 的使用者物件上編輯屬性。

  群組應該遵循標準命名慣例而且只用於工作資料夾，以避免產生與其他安全性需求的可能衝突。

  若要建立適當的安全性群組，請多次使用下列程序：一次是針對每個同步共用，一次是選擇性的為檔案伺服器系統管理員建立群組。

#### <a name="to-create-security-groups-for-work-folders"></a>建立工作資料夾的安全性群組

1. 在已安裝「Active Directory 系統管理中心」的 Windows Server 2012 R2 或 Windows Server 2016 電腦上，開啟 \[伺服器管理員\]。

2.  在 [工具] 功能表上，按一下 [Active Directory 系統管理中心]。 \[Active Directory 系統管理中心\] 隨即顯示。

3.  在想要建立新群組的容器上按滑鼠右鍵 (例如，適當網域或 OU 的使用者容器)，按一下 **\[新增\]** ，然後按一下 **\[群組\]** 。

4.  在 [建立群組] 視窗內的 [群組] 區段中，指定下列設定：

    -   在 **\[群組名稱\]** 中，輸入安全性群組的名稱，例如：**HR Sync Share Users** 或**工作資料夾系統管理員**。  
  
    -   在 [群組領域]中，按一下 [安全性]，然後按一下 [全域]。  
  
5.  在 [成員] 區段中，按一下 [新增]。 \[選取使用者、連絡人、電腦、服務帳戶或群組\] 對話方塊隨即顯示。  
  
6.  輸入您要授與存取特定同步共用權限的使用者或群組的名稱 (如果您要建立群組來控制同步共用存取)，或輸入工作資料夾系統管理員的名稱 (如果您要設定使用者帳戶以自動探索適當同步伺服器)，按一下 **\[確定\]** ，然後再按一下 **\[確定\]** 。

若要使用 Windows PowerShell 建立安全性群組，請使用下列 Cmdlet：

```powershell  
$GroupName = "Work Folders Administrators"  
$DC = "DC1.contoso.com"  
$ADGroupPath = "CN=Users,DC=contoso,DC=com"  
$Members = "CN=Maya Bender,CN=Users,DC=contoso,DC=com","CN=Irwin Hume,CN=Users,DC=contoso,DC=com"  
  
New-ADGroup -GroupCategory:"Security" -GroupScope:"Global" -Name:$GroupName -Path:$ADGroupPath -SamAccountName:$GroupName -Server:$DC  
Set-ADGroup -Add:@{'Member'=$Members} -Identity:$GroupName -Server:$DC
```

## <a name="step-6-optionally-delegate-user-attribute-control-to-work-folders-administrators"></a>步驟 6 ：選擇性將使用者屬性控制項委派給工作資料夾系統管理員  
 如果您要部署多個同步伺服器，希望自動將使用者引導到正確的同步伺服器，就必須更新 AD DS 中每個使用者帳戶上的屬性。 不過，這通常需要取得 Domain Admins 或 Enterprise Admins 群組的成員才能更新屬性，如果您需要經常新增使用者或在同步伺服器之間移動使用者的話，很快就會對這種做法感到疲累。  
  
 因此，Domain Admins 或 Enterprise Admins 群組的成員可能會希望將修改使用者物件之 msDS-SyncServerURL 屬性的能力，委派給您在步驟 5 中建立的工作資料夾系統管理員群組，如下面的程序中所述。  
  
#### <a name="delegate-the-ability-to-edit-the-msds-syncserverurl-property-on-user-objects-in-ad-ds"></a>委派編輯 AD DS 中使用者物件上 msDS-SyncServerURL 屬性的能力  
  
1.  在已安裝「Active Directory 使用者和電腦」的 Windows Server 2012 R2 或 Windows Server 2016 電腦上，開啟 \[伺服器管理員\]。  
  
2.  在 **\[工具\]** 功能表上，按一下 **\[Active Directory 使用者和電腦\]** 。 \[Active Directory 使用者和電腦\] 隨即顯示。  
  
3.  在其中包含工作資料夾所有使用者物件的 OU 上按滑鼠右鍵 (如果使用者儲存於多個 OU 或網域中，請在所有使用者通用的容器上按滑鼠右鍵)，然後按一下 **\[委派控制...\]** 。 \[委派控制精靈\] 隨即顯示。  
  
4.  在 **\[使用者或群組\]** 頁面上，按一下 **\[新增\]** 。 然後為工作資料夾系統管理員指定您建立的群組 (例如**工作資料夾系統管理員**)。  
  
5.  在 **\[將委派的工作\]** 頁面上，按一下 **\[建立自訂工作來委派\]** 。  
  
6.  在 **\[Active Directory 物件類型\]** 頁面上，按一下 **\[只有在這個資料夾內的下列物件\]** ，然後選取 **\[使用者物件\]** 核取方塊。  
  
7.  在 **\[權限\]** 頁面上，清除 **\[一般\]** 核取方塊，選取 **\[內容特定\]** 核取方塊，然後選取 **\[讀取 msDS-SyncServerUrl\]** 和 **\[寫入 msDS-SyncServerUrl\]** 核取方塊。

若要使用 Windows PowerShell 委派編輯使用者物件上 msDS-SyncServerURL 屬性的能力，請使用下列使用 DsAcls 命令的範例指令碼。
  
```powershell  
$GroupName = "Contoso\Work Folders Administrators"  
$ADGroupPath = "CN=Users,dc=contoso,dc=com"  
  
DsAcls $ADGroupPath /I:S /G ""$GroupName":RPWP;msDS-SyncServerUrl;user"  
```  
  
> [!NOTE]
>  委派作業在包含大量使用者的網域中執行時可能需要一些時間。  
  
## <a name="step-7-create-sync-shares-for-user-data"></a>步驟 7：建立使用者資料的同步共用  
 此時，您已經可以開始指定同步伺服器的資料夾來存放使用者的檔案。 這個資料夾稱為同步共用，您可以使用下列程序來建立該資料夾。  
  
1. 如果您還沒有 NTFS 磁碟區可以提供容納同步共用及使用者檔案的可用空間，請建立新的磁碟區並使用 NTFS 檔案系統加以格式化。  
  
2. 在 \[伺服器管理員\] 中，按一下 **\[檔案和存放服務\]** ，然後按一下 **\[工作資料夾\]** 。  
  
3. 在詳細資料窗格的最上方可看到任何現有同步共用的清單。 若要建立新的同步共用，請從 **\[工作\]** 功能表選擇 **\[新增同步共用...\]** 。 \[新增同步共用精靈\] 隨即顯示。  
  
4. 在 **\[選取伺服器與路徑\]** 頁面上，指定存放同步共用的位置。 如果您已經為此使用者資料建立檔案共用，可以選擇該共用。 或者，也可以建立新的資料夾。  
  
   > [!NOTE]
   >  根據預設，無法透過檔案共用直接存取同步共用 (除非您挑選現有的檔案共用) 如果您希望讓同步共用可以透過檔案共用存取，請使用伺服器管理員的 **\[共用\]** 磚或 [New-SmbShare](https://technet.microsoft.com/library/jj635722.aspx) Cmdlet 來建立檔案共用，最好是啟用存取型列舉。  
  
5. 在 **\[指定使用者資料夾的結構\]** 頁面上，為同步共用內的使用者資料夾選擇命名慣例。 其中提供兩個選項：  
  
   - **\[使用者別名\]** 會建立不包含網域名稱的使用者資料夾。 如果您是使用已經搭配使用資料夾重新導向或其他使用者資料解決方案的檔案共用，請選取此命名慣例。 您可以選擇性地選取 **\[只同步下列子資料夾\]** 核取方塊，僅對特定子資料夾 (例如 \[文件\] 資料夾) 同步處理。  
  
   - <strong>\[使用者 alias@domain\]</strong> 會建立包含網域名稱的使用者資料夾。 如果使用的不是已經用於資料夾重新導向或其他使用者資料解決方案的檔案共用，請選取此命名慣例，以避免在共用的多個使用者具有相同別名時產生資料夾命名衝突 (這可能會在使用者屬於不同網域的情況下發生)。  
  
6. 在 **\[輸入同步共用名稱\]** 頁面上，指定同步共用的名稱和描述。 這不會在網路上通告，但是會顯示在伺服器管理員和 Windows Powershell 中，以協助辨別各個同步共用。  
  
7. 在 **\[將同步存取權授與群組\]** 頁面上，指定之前建立且列示允許使用此同步共用之使用者的群組。  
  
   > [!IMPORTANT]
   >  若要提高效能和安全性，請將存取權授與群組而不是個別使用者，而且盡可能明確，避免使用通用群組 (如 Authenticated Users 和 Domain Users)。 將存取權授與包含大量使用者的群組，會讓工作資料夾耗費很長時間來查詢 AD DS。 如果您有大量使用者，請建立多個同步共用來協助分散負載。  
  
8. 在 **\[指定裝置原則\]** 頁面上，指定是否在用戶端電腦和裝置上要求任何安全性限制。 有兩個個別的裝置原則可以選擇：  
  
   -   **\[加密工作資料夾\]** 要求加密用戶端電腦和裝置上的工作資料夾  
  
   -   **\[自動鎖定畫面，並要求輸入密碼\]** 要求用戶端電腦和裝置在 15 分鐘後自動鎖定螢幕，要求輸入六個字元或更多的密碼來解除鎖定螢幕，並在重試 10 次失敗之後啟動裝置鎖定模式  
  
       > [!IMPORTANT]
       >  若要為 Windows 7 電腦與已加入網域之電腦的非系統管理員使用者強制套用密碼原則，請為電腦網域使用群組原則密碼原則，並將這些網域從「工作資料夾」密碼原則排除。 排除網域的方法，是在建立同步共用之後使用 [Set-Syncshare -PasswordAutoExcludeDomain](https://technet.microsoft.com/library/dn296649\(v=wps.630\).aspx) Cmdlet。 如需設定群組原則密碼原則的相關資訊，請參閱[密碼原則](https://technet.microsoft.com/library/hh994572(v=ws.11).aspx)。  
  
9. 檢閱您的選項並完成精靈以建立同步共用。

使用 Windows PowerShell 建立同步共用的方法是使用 [New-SyncShare](https://technet.microsoft.com/library/dn296635.aspx) Cmdlet。 以下是這個方法的範例：  
  
```powershell  
New-SyncShare "HR Sync Share" K:\Share-1 –User "HR Sync Share Users"  
```  
  
上面的範例會建立一個新的同步共用，名稱為 *Share01*，路徑為 *K:\Share-1*，並將存取權授與給名為 *HR Sync Share Users* 的群組。  
  
> [!TIP]
>  建立同步共用之後，可以使用檔案伺服器資源管理員功能來管理共用中的資料。 例如，您可以使用伺服器管理員中 \[工作資料夾\] 頁面中的 **\[配額\]** 磚，設定使用者資料夾上的配額。 您也可以使用[檔案檢測管理](https://technet.microsoft.com/library/cc732074.aspx)控制工作資料夾要同步的檔案類型，或是使用[動態存取控制](https://technet.microsoft.com/windows-server-docs/identity/solution-guides/dynamic-access-control--scenario-overview)中所述的案例，以進行較為複雜的檔案分類工作。  
  
## <a name="step-8-optionally-specify-a-tech-support-email-address"></a>步驟 8：選擇性指定技術支援電子郵件地址   
 將工作資料夾安裝在檔案伺服器之後，您或許會想要指定伺服器的系統管理連絡人電子郵件地址。 若要新增電子郵件地址，請使用下列程序：  
  
#### <a name="specifying-an-administrative-contact-email"></a>指定系統管理連絡人電子郵件 
  
1.  在 \[伺服器管理員\] 中，按一下 **\[檔案和存放服務\]** ，然後按一下 **\[伺服器\]** 。  
  
2.  在同步伺服器上按滑鼠右鍵，再按一下 **\[工作資料夾設定\]** 。 \[工作資料夾設定\] 視窗隨即顯示。  
  
3.  在瀏覽窗格中，按一下 **\[支援電子郵件\]** ，然後輸入電子郵件地址或是寄送工作資料夾方面的協助時使用者應該使用的電子郵件地址。 完成後，按一下 **\[確定\]** 。  
  
     工作資料夾使用者可以按一下 \[工作資料夾控制台\] 項目中的連結，將包含用戶端電腦診斷資訊的電子郵件，傳送到您在這裡指定的地址。  
  
## <a name="step-9-optionally-set-up-server-automatic-discovery"></a>步驟 9：選擇性設定伺服器自動探索  
 如果您的環境中裝載了多個同步伺服器，則應該在 AD DS 中使用者帳戶上填入 **msDS-SyncServerURL** 屬性來設定伺服器自動探索。  
  
>[!NOTE]
>要透過反向 Proxy 解決方案 (例如 Web 應用程式 Proxy 或 Azure AD 應用程式 Proxy) 存取工作資料夾的遠端使用者不可定義 Active Directory 中的 msDS-SyncServerURL 屬性。 如果已定義 Msds-syncserverurl 屬性，則工作資料夾用戶端會嘗試存取無法透過反向 proxy 解決方案存取的內部 URL。 使用 Web 應用程式 Proxy 或 Azure AD 應用程式 Proxy 時，您必須為每一部工作資料夾伺服器建立唯一 Proxy 應用程式。 如需詳細資訊，請參閱[搭配 AD FS 與 Web 應用程式 Proxy 部署工作資料夾：概觀](deploy-work-folders-adfs-overview.md)或[搭配 Azure AD 應用程式 Proxy 部署工作資料夾](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/)。


 執行這個動作之前，您必須先安裝 Windows Server 2012 R2 網域控制站或使用 `Adprep /forestprep` 和 `Adprep /domainprep` 命令更新樹系和網域結構描述。 如需如何安全地執行這些命令的相關資訊，請參閱[執行 Adprep](https://technet.microsoft.com/library/dd464018.aspx)。  
  
 您可能也要為檔案伺服器系統管理員建立安全性群組，並授與他們修改此特定使用者屬性的委派權限，如步驟 5 和步驟 6 中所述。 如果不執行這些步驟，您就必須讓 Domain Admins 或 Enterprise Admins 群組的成員為每個使用者設定自動探索。  
  
#### <a name="to-specify-the-sync-server-for-users"></a>為使用者指定同步伺服器  
  
1.  在已安裝 Active Directory 系統管理工具的電腦上開啟伺服器管理員。  
  
2.  在 [工具] 功能表上，按一下 [Active Directory 系統管理中心]。 \[Active Directory 系統管理中心\] 隨即顯示。  
  
3.  瀏覽到適當網域中的 **\[使用者\]** 容器，在想要指派給同步共用的使用者上按滑鼠右鍵，再按一下 **\[內容\]** 。  
  
4.  在瀏覽窗格中，按一下 **\[延伸\]** 。  
  
5.  按一下 **\[屬性編輯器\]** 索引標籤，選取 **\[msDS-SyncServerUrl\]** ，然後按一下 **\[編輯\]** 。 \[多重字串值編輯器\] 對話方塊隨即顯示。  
  
6.  在 **\[要新增的值\]** 方塊中，輸入想要此使用者與其同步的同步伺服器 URL，依序按一下 **\[新增\]** 、 **\[確定\]** ，然後再按一下 **\[確定\]** 。  
  
    > [!NOTE]
    >  同步伺服器 URL 只是 `https://` 或 `http://` (取決於是否想要求安全連線)，後面是同步伺服器的完整網域名稱。 例如， **HTTPs：\//sync1.contoso.com**。

若要填入多個使用者的屬性，請使用 Active Directory PowerShell。 下面是為 *HR Sync Share Users* 群組的所有成員填入屬性的範例，在步驟 5 中已討論過。
  
```powershell  
$SyncServerURL = "https://sync1.contoso.com"  
$GroupName = "HR Sync Share Users"  
  
Get-ADGroupMember -Identity $GroupName |  
Set-ADUser –Add @{"msDS-SyncServerURL"=$SyncServerURL}  
  
```  
  
## <a name="step-10-optionally-configure-web-application-proxy-azure-ad-application-proxy-or-another-reverse-proxy"></a>步驟 10：選擇性設定 Web 應用程式 Proxy、Azure AD 應用程式 Proxy 或其他反向 Proxy  

若要允許遠端使用者存取其檔案，您必須透過反向 Proxy 發佈工作資料夾伺服器，讓工作資料夾可在網際網路上供外部存取。 您可以使用 Web 應用程式 Proxy、Azure Active Directory 應用程式 Proxy 或其他反向 Proxy 解決方案。  
  
如需使用 AD FS and Web 應用程式 Proxy 來設定工作資料夾存取，請參閱[搭配 AD FS 與 Web 應用程式 Proxy (WAP) 部署工作資料夾](deploy-work-folders-adfs-overview.md)。 如需 Web 應用程式 Proxy 的背景資訊，請參閱 [Windows Server 2016 中的 Web 應用程式 Proxy](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server)。 如需有關使用「Web 應用程式 Proxy」在網際網路上發佈應用程式 (例如「工作資料夾」) 的詳細資訊，請參閱[使用 AD FS 預先驗證發佈應用程式](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/publishing-applications-using-ad-fs-preauthentication)。
 
如需使用 Azure Active Directory 應用程式 Proxy 來設定工作資料夾存取，請參閱[使用 Azure Active Directory 應用程式 Proxy 啟用工作資料夾遠端存取](https://blogs.technet.microsoft.com/filecab/?p=7945) 
  
## <a name="step-11-optionally-use-group-policy-to-configure-domain-joined-pcs"></a>步驟 11：選擇性使用群組原則設定加入網域的電腦  

如果您有大量加入網域的電腦需要部署工作資料夾，可使用群組原則執行下列用戶端電腦設定工作：  
  
- 指定要與其同步的同步伺服器使用者  
  
- 使用預設設定強制自動設定工作資料夾 (進行這個動作之前，請參閱[設計工作資料夾實作](plan-work-folders.md)中關於群組原則的討論)。  
  
  若要控制這些設定，請為工作資料夾建立新的群組原則物件 (GPO)，然後適當地設定下列群組原則設定：  
  
- 使用者設定\原則\系統管理範本\Windows 元件\WorkFolders 中的「指定 Work Folders 設定」原則設定  
  
- 電腦設定\原則\系統管理範本\Windows 元件\WorkFolders 中的「強制為所有使用者自動設定」原則設定  
  
> [!NOTE]
>  只有從在 Windows 8.1、Windows Server 2012 R2 或更新版本上執行 \[群組原則管理\] 的電腦上編輯群組原則時，才能使用這些原則設定。 舊版作業系統的群組原則管理版本不提供此設定。 這些原則設定適用於已安裝[適用於 Windows 7 的工作資料夾](https://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx)應用程式的 Windows 7 電腦。  
  
##  <a name="BKMK_LINKS"></a> 另請參閱  
 如需其他相關資訊，請參閱下列資源。  
  
|內容類型|參考資料|  
|------------------|----------------|  
|**理解**|-   [工作資料夾](work-folders-overview.md)|  
|**規劃**|-   [設計工作資料夾的執行](plan-work-folders.md)|
|**部署**|-   [使用 AD FS 和 Web 應用程式 Proxy （WAP）部署工作資料夾](deploy-work-folders-adfs-overview.md)<br />-   [工作資料夾測試實驗室部署](https://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx)（blog 文章）<br />-   [工作資料夾伺服器 Url 的新使用者屬性](https://blogs.technet.com/b/filecab/archive/2013/10/09/a-new-user-attribute-for-work-folders-server-url.aspx)（blog 文章）|  
|**技術參考資料**|-   [互動式登入：電腦帳戶鎖定閾值](https://technet.microsoft.com/library/jj966264(v=ws.11).aspx)<br />-   [同步共用 Cmdlet](https://docs.microsoft.com/powershell/module/syncshare/?view=win10-ps)|

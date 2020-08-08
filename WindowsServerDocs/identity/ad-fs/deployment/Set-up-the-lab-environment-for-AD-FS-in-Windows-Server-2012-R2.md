---
ms.assetid: 6b38480e-5b1c-49f0-9d46-8cf22f70f0d2
title: 在 Windows Server 2012 R2 設定 AD FS 實驗室環境
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 276c4d22c3df64debd696ae07fff996fdbf4ceea
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994279"
---
# <a name="set-up-the-lab-environment-for-ad-fs-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 設定 AD FS 實驗室環境


此主題概述設定測試環境以用於完成下列逐步解說指南中之逐步解說所需的步驟：

-   [逐步解說：將 iOS 裝置加入工作地點網路](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

-   [逐步解說：將 Windows 裝置加入工作地點網路](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)


-   [逐步解說指南：使用條件式存取控制管理風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)

-   [逐步解說指南：透過其他多因素驗證管理機密應用程式的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

> [!NOTE]
> 建議您不要將網頁伺服器與同盟伺服器安裝在同一部電腦上。

若要設定使測試環境，請完成下列步驟：

1.  [步驟 1：設定網域控制站 (DC1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_1)

2.  [步驟2：使用裝置註冊服務設定同盟伺服器 (ADFS1) ](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)

3.  [步驟 3：設定網頁伺服器 (WebServ1) 與範例宣告式應用程式](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_5)

4.  [步驟 4：設定用戶端電腦 (Client1)](../../ad-fs/deployment/../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_10)

## <a name="step-1-configure-the-domain-controller-dc1"></a><a name="BKMK_1"></a>步驟 1：設定網域控制站 (DC1)
基於此測試環境的目的，您可以呼叫根 Active Directory 網域**contoso.com** ，並指定 <strong>pass@word1</strong> 做為系統管理員密碼。

-   安裝 AD DS 角色服務，並安裝 Active Directory Domain Services (AD DS) ，讓您的電腦成為 Windows Server 2012 R2 中的網域控制站。 此動作會在建立網域控制站的過程中升級您的 AD DS 架構。 如需詳細資訊和逐步指示，請參閱 [https://technet.microsoft.com/library/hh472162.aspx](../../ad-ds/deploy/install-active-directory-domain-services--level-100-.md) 。

### <a name="create-test-active-directory-accounts"></a><a name="BKMK_2"></a>建立測試 Active Directory 帳戶
在您的網域控制站開始運作之後，您可以在此網域中建立測試群組與測試使用者帳戶，並將該使用者帳戶新增到群組帳戶。 您可以使用這些帳戶來完成此主題稍早所述之逐步解說指南中的逐步解說。

建立下列帳戶：

- 使用者： **Robert Hatley**具有下列認證：使用者名稱： **RobertH**和密碼：<strong>P@ssword</strong>

- 群組︰ **Finance**

如需如何在 Active Directory (AD) 中建立使用者和群組帳戶的相關資訊，請參閱 [https://technet.microsoft.com/library/cc783323%28v.aspx](/previous-versions/windows/it-pro/windows-server-2003/cc783323(v=ws.10)) 。

新增 **Robert Hatley** 帳戶至 **Finance** 群組。 如需如何在 Active Directory 中將使用者新增至群組的詳細資訊，請參閱 [https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx](/previous-versions/windows/it-pro/windows-server-2003/cc737130(v=ws.10)) 。

### <a name="create-a-gmsa-account"></a>建立 GMSA 帳戶
在 Active Directory 同盟服務 (AD FS) 安裝和設定期間，需要群組受管理的服務帳戶 (GMSA) 帳戶。

##### <a name="to-create-a-gmsa-account"></a>建立 GMSA 帳戶

1.  開啟 Windows PowerShell 命令視窗並輸入：

    ```
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    New-ADServiceAccount FsGmsa -DNSHostName adfs1.contoso.com -ServicePrincipalNames http/adfs1.contoso.com

    ```

## <a name="step-2-configure-the-federation-server-adfs1-by-using-device-registration-service"></a><a name="BKMK_4"></a>步驟 2：使用「裝置註冊服務」設定同盟伺服器 (ADFS1)
若要設定另一部虛擬機器，請安裝 Windows Server 2012 R2，並將它連線到網域**contoso.com**。 將電腦加入網域之後，請進行設定，然後繼續安裝並設定 AD FS 角色。

如需影片，請參閱 [Active Directory Federation Services How-To Video Series: Installing an AD FS Server Farm](https://channel9.msdn.com/Search?term=Active%20Directory%20Federation%20Services#pubDate=year&ch9Search)(Active Directory 同盟服務使用方法影片系列：安裝 AD FS 伺服器陣列)。

### <a name="install-a-server-ssl-certificate"></a>安裝伺服器 SSL 憑證
您必須在 ADFS1 伺服器上的本機電腦存放區中安裝伺服器安全通訊端層 (SSL) 憑證。 該憑證必須有下列屬性：

-   主體名稱 (CN)：adfs1.contoso.com

-   主體別名 (DNS)：adfs1.contoso.com

-   主體別名 (DNS)：enterpriseregistration.contoso.com

如需有關設定 SSL 憑證的詳細資訊，請參閱 [在具有企業 CA 之網域中的網站上設定 SSL/TLS](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831822(v=ws.11))。

[Active Directory Federation Services How-To Video Series: Updating Certificates](https://channel9.msdn.com/Search?term=Active%20Directory%20Federation%20Services#pubDate=year&ch9Search)(Active Directory 同盟服務使用方法影片系列：更新憑證)。

### <a name="install-the-ad-fs-server-role"></a>安裝 AD FS 伺服器角色

##### <a name="to-install-the-federation-service-role-service"></a>安裝 Federation Service 角色服務

1. 使用網域系統管理員帳戶登入伺服器 administrator@contoso.com 。

2. 啟動伺服器管理員。 若要啟動 [伺服器管理員]，請按一下 Windows [開始]**** 畫面上的 [伺服器管理員]****，或按一下 Windows 桌面上 Windows 工作列中的 [伺服器管理員]****。 在 [儀表板]**** 頁面之 [歡迎]**** 區段的 [快速入門]**** 索引標籤中，按一下 [新增角色及功能]****。 或者，您可以按一下 [管理]**** 功能表中的 [新增角色及功能]****。

3. 在 [在您開始前]  頁面上，按一下 [下一步]  。

4. 在 [選取安裝類型]**** 頁面上，按一下 [角色型或功能型安裝]****，然後按 [下一步]****。

5. 在 [選取目的地伺服器]**** 頁面上，按一下 [從伺服器集區選取伺服器]****，確認已選取目標電腦，然後按一下 [下一步]****。

6. 在 [選取伺服器角色]**** 頁面上，按一下 [Active Directory Federation Services]****，然後按 [下一步]****。

7. 在 [選取功能]**** 頁面上，按 [下一步]****。

8. 在 [Active Directory Federation Service (AD FS)]**** 頁面中，按 [下一步]****。

9. 在您驗證 [確認安裝選項]**** 頁面上的資訊之後，請選取 [必要時自動重新啟動目的地伺服器]**** 核取方塊，然後按一下 [安裝]****。

10. 在 [安裝進度]**** 頁面中，確認所有項目均已順利安裝，然後按一下 [關閉]****。

### <a name="configure-the-federation-server"></a>設定同盟伺服器
下一個步驟是設定同盟伺服器。

##### <a name="to-configure-the-federation-server"></a>設定同盟伺服器

1.  在伺服器管理員的 [儀表板]**** 頁面上，按一下 [通知]**** 旗標，然後按一下 [在伺服器上設定同盟服務]****。

    [Active Directory Federation Service 設定精靈]**** 隨即開啟。

2.  在 [歡迎]**** 頁面上，選取 [在同盟伺服器陣列中建立第一部同盟伺服器]****，然後按一下 [下一步]****。

3.  在 [連線到 AD DS]**** 頁面上，指定具有此電腦已加入之 **contoso.com** Active Directory 網域之網域系統管理員權限的帳戶，然後按一下 [下一步]****。

4.  在 [指定服務內容]**** 頁面上執行下列動作，然後按一下 [下一步]****：

    -   匯入您稍早取得的 SSL 憑證。 此憑證是必要的服務驗證憑證。 瀏覽到您的 SSL 憑證所在位置。

    -   若要為您的 Federation Service 提供名稱，請輸入 **adfs1.contoso.com**。 此值與您在 Active Directory 憑證服務 (AD CS) 中註冊 SSL 憑證時提供的值一樣。

    -   若要為您的 Federation Service 提供顯示名稱，請輸入 **Contoso Corporation**。

5.  在 [指定服務帳戶]**** 頁面上，選取 [使用現有的網域使用者帳戶或群組「受管理的服務帳戶」]****，然後指定您在建立網域控制站時建立的 GMSA 帳戶 **fsgmsa**。

6.  在 [指定設定資料庫]**** 頁面上，選取 [在此伺服器上使用 Windows 內部資料庫來建立資料庫]****，然後按一下 [下一步]****。

7.  在 [檢閱選項]**** 頁面上，檢查您的設定選項，然後按一下 [下一步]****。

8.  在 [先決條件檢查]**** 頁面上，確認所有先決條件檢查都已順利完成，然後按一下 [設定]****。

9. 在 [結果]**** 頁面上，檢閱結果並檢查設定是否已順利完成，然後按一下 [完成 Federation Service 部署所需的後續步驟]****。

### <a name="configure-device-registration-service"></a>設定裝置註冊服務
接下來的步驟是在 ADFS1 伺服器上設定「裝置註冊服務」。 如需影片，請參閱 [Active Directory Federation Services How-To Video Series: Enabling the Device Registration Service](https://channel9.msdn.com/)(Active Directory 同盟服務使用方法影片系列：啟用裝置註冊服務)。

##### <a name="to-configure-device-registration-service-for-windows-server-2012-rtm"></a>為 Windows Server 2012 RTM 設定裝置註冊服務

1.  > [!IMPORTANT]
    > **下列步驟適用於 Windows Server 2012 R2 RTM 版本。**

    開啟 Windows PowerShell 命令視窗並輸入：

    ```
    Initialize-ADDeviceRegistration
    ```

    當系統提示您提供服務帳戶時，請輸入 **contoso\fsgmsa$**。

    現在，請執行 Windows PowerShell Cmdlet。

    ```
    Enable-AdfsDeviceRegistration
    ```

2.  在 ADFS1 伺服器上，在 [AD FS 管理]**** 主控台上，瀏覽到 [驗證原則]****。 選取 [啟用全域主要驗證]****。 選取 [啟用裝置驗證]****，然後按一下 [確定]****。

### <a name="add-host-a-and-alias-cname-resource-records-to-dns"></a>將主機 (A) 與別名 (CNAME) 資源記錄新增至 DNS
在 DC1 上，您必須確定已針對「裝置註冊服務」建立下列網域名稱系統 (DNS) 記錄。

|進入|類型|位址|
|---------|--------|-----------|
|adfs1|主機 (A)|AD FS 伺服器的 IP 位址|
|enterpriseregistration|別名 (CNAME)|adfs1.contoso.com|

您可以使用下列程序，針對同盟伺服器與「裝置註冊服務」將主機 (A) 資源記錄到新增公司 DNS 名稱伺服器。

若要完成此程序，至少需要 Administrators 群組的成員資格或同等權限。 如需使用適當帳戶和群組成員資格的詳細資訊，請參閱超連結 " <https://go.microsoft.com/fwlink/?LinkId=83477> " 本機和網域預設群組 (<https://go.microsoft.com/fwlink/p/?LinkId=83477>) 。

##### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>為您的同盟伺服器將主機 (A) 與別名 (CNAME) 資源記錄新增至 DNS

1.  在 DC1 上，從 [伺服器管理員] 的 [工具]**** 功能表，按一下 [DNS]**** 以開啟 [DNS] 嵌入式管理單元。

2.  在主控台樹狀目錄中，展開 [正向對應區域]****，在 [contoso.com]**** 上按一下滑鼠右鍵，然後按一下 [新增主機 (A 或 AAAA)]****。

3.  在 [名稱]**** 中，輸入您要為 AD FS 陣列使用的名稱。 對於此逐步解說，請輸入 **adfs1**。

4.  在 [IP 位址]**** 中，輸入 ADFS1 伺服器的 IP 位址。 按一下 [新增主機]****。

5.  在 [contoso.com]**** 上按一下滑鼠右鍵，然後按一下 [新增別名 (CNAME)]****。

6.  在 [新增資源記錄]**** 對話方塊中，在 [別名名稱]**** 方塊中輸入 **enterpriseregistration**。

7.  在 [目標主機完整網域名稱 (FQDN)] 方塊中，輸入 **adfs1.contoso.com**，然後按一下 [確定]****。

    > [!IMPORTANT]
    > 在真實世界的部署中，若您的公司有多個使用者主體名稱 (UPN) 尾碼，您必須建立多個 CNAME 記錄 (每個記錄適用於 DNS 中的一個 UPN 尾碼)。

## <a name="step-3-configure-the-web-server-webserv1-and-a-sample-claims-based-application"></a><a name="BKMK_5"></a>步驟 3：設定網頁伺服器 (WebServ1) 與範例宣告式應用程式
藉由安裝 Windows Server 2012 R2 作業系統並將它連線到網域**contoso.com**， (WebServ1) 設定虛擬機器。 當它加入該網域後，您可以繼續安裝及設定「網頁伺服器」角色。

若要完成此主題稍早所述的逐步解說，您必須有受您的同盟伺服器 (ADFS1) 所保護的簡單應用程式。

您可以下載 Windows Identity Foundation SDK ([https://www.microsoft.com/download/details.aspx?id=4451](https://www.microsoft.com/download/details.aspx?id=4451) ，其中包括以宣告為基礎的範例應用程式。

您必須完成下列步驟，才能使用此宣告式應用程式來設定網頁伺服器。

> [!NOTE]
> 這些步驟已在執行 Windows Server 2012 R2 作業系統的網頁伺服器上測試過。

1.  [安裝網頁伺服器角色與 Windows Identity Foundation](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_15)

2.  [安裝 Windows Identity Foundation SDK](../../ad-fs/deployment/../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_13)

3.  [在 IIS 中設定簡單宣告應用程式](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_9)

4.  [在您的同盟伺服器建立信賴憑證者信任](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_11)

### <a name="install-the-web-server-role-and-windows-identity-foundation"></a><a name="BKMK_15"></a>安裝網頁伺服器角色與 Windows Identity Foundation

1. > [!NOTE]
   > 您必須具有 Windows Server 2012 R2 安裝媒體的存取權。

   使用和密碼登入 WebServ1 <strong>administrator@contoso.com</strong> <strong>pass@word1</strong> 。

2. 在 [伺服器管理員] 中，在 [儀表板]**** 頁面 之 [歡迎]**** 區段的 [快速入門]**** 索引標籤中，按一下 [新增角色及功能]****。 或者，您可以按一下 [管理]**** 功能表中的 [新增角色及功能]****。

3. 在 [在您開始前]  頁面上，按一下 [下一步]  。

4. 在 [選取安裝類型]**** 頁面上，按一下 [角色型或功能型安裝]****，然後按 [下一步]****。

5. 在 [選取目的地伺服器]**** 頁面上，按一下 [從伺服器集區選取伺服器]****，確認已選取目標電腦，然後按一下 [下一步]****。

6. 在 [選取伺服器角色]**** 頁面上，選取 [網頁伺服器 (IIS)]**** 旁的核取方塊，按一下 [新增功能]****，然後按一下 [下一步]****。

7. 在 [選取功能]**** 頁面上，選取 [Windows Identity Foundation 3.5]****，然後按一下 [下一步]****。

8. 在 [網頁伺服器 (IIS) 角色]**** 頁面上，按一下 [下一步]****。

9. 在 [選取角色服務]**** 頁面上，選取並展開 [應用程式開發]****。 選取 [ASP.NET 3.5]****，按一下 [新增功能]****，然後按一下 [下一步]****。

10. 在 [確認安裝選項]**** 頁面上，按一下 [指定替代來源路徑]****。 輸入位於 Windows Server 2012 R2 安裝媒體中 Sxs 目錄的路徑。 例如 D:\Sources\Sxs。 按一下 [確定]****，然後按一下 [安裝]****。

### <a name="install-windows-identity-foundation-sdk"></a><a name="BKMK_13"></a>安裝 Windows Identity Foundation SDK

1.  執行 WindowsIdentityFoundation-SDK-3.5.msi 以安裝 Windows Identity Foundation SDK 3.5 (https://www.microsoft.com/download/details.aspx?id=4451) 。 選擇所有預設選項。

### <a name="configure-the-simple-claims-app-in-iis"></a><a name="BKMK_9"></a>在 IIS 中設定簡單宣告應用程式

1.  在電腦憑證存放區中安裝有效的 SSL 憑證。 憑證應該包含您的網頁伺服器名稱 **webserv1.contoso.com**。

2.  將 C:\Program Files (x86)\Windows Identity Foundation SDK\v3.5\Samples\Quick Start\Web Application\PassiveRedirectBasedClaimsAwareWebApp 的內容複製到 C:\Inetpub\Claimapp。

3.  編輯 **Default.aspx.cs** 檔案並停用宣告篩選。 執行此步驟的目的是要確定範例應用程式會顯示同盟伺服器發出的所有宣告。 執行下列動作：

    1.  在文字編輯器中開啟 **Default.aspx.cs**。

    2.  搜尋該檔案中的第二個 `ExpectedClaims`執行個體。

    3.  將整個 `IF` 陳述式及其大括弧變更為註解。 輸入 "//" (，並在行首的) 不加引號，表示批註。

    4.  您的 `FOREACH` 陳述式現在看起來應該像此程式碼範例一樣。

        ```
        Foreach (claim claim in claimsIdentity.Claims)
        {
           //Before showing the claims validate that this is an expected claim
           //If it is not in the expected claims list then don't show it
           //if (ExpectedClaims.Contains( claim.ClaimType ) )
           // {
              writeClaim( claim, table );
           //}
        }

        ```

    5.  儲存並關閉 **Default.aspx.cs**。

    6.  在文字編輯器中開啟 **web.config**。

    7.  移除整個 `<microsoft.identityModel>` 區段。 移除從 `including <microsoft.identityModel>` 一直到 (並包含) `</microsoft.identityModel>`之間的所有內容。

    8.  儲存並關閉 **web.config**。

4.  **設定 IIS 管理員**

    1.  開啟 [Internet Information Services (IIS) 管理員] 。

    2.  移至 [應用程式集區]****，在 [DefaultAppPool]**** 上按一下滑鼠右鍵，然後選取 [進階設定]****。 將 [載入使用者設定檔]**** 設定為 [True]****，然後按一下 [確定]****。

    3.  在 [DefaultAppPool]**** 上按一下滑鼠右鍵，然後選取 [基本設定]****。 將 [.NET CLR 版本]**** 變更為 [.NET CLR 版本 v2.0.50727]****。

    4.  在 [預設的網站]**** 上按一下滑鼠右鍵，然後選取 [編輯繫結]****。

    5.  使用您已安裝的 SSL 憑證，新增 **HTTPS** 繫結至 **443**。

    6.  在 [預設的網站]**** 上按一下滑鼠右鍵，然後選取 [新增應用程式]****。

    7.  將別名設定為 **claimapp** 並將實體路徑設定為 **c:\inetpub\claimapp**。

5.  若要設定 **claimapp** 搭配您的同盟伺服器使用，請執行下列動作：

    1.  執行 FedUtil.exe，它位於 **C:\Program Files (x86)\Windows Identity Foundation SDK\v3.5**。

    2.  將應用程式設定位置設為**C:\inetput\claimapp\web.config** ，並將應用程式 URI 設為您網站的 URL ** https://webserv1.contoso.com /claimapp/**。 按 [下一步]  。

    3.  選取 [**使用現有的 STS** ]，然後流覽至您的 AD FS 伺服器的中繼資料 URL **https://adfs1.contoso.com/federationmetadata/2007-06/federationmetadata.xml** 。 按 [下一步]  。

    4.  選取 [停用憑證鏈結驗證]****，然後按一下 [下一步]****。

    5.  選取 [無加密]****，然後按一下 [下一步]****。 在 [提供的宣告]**** 頁面上，按一下 [下一步]****。

    6.  選取 [排定工作以執行每日 WS-Federation 中繼資料更新]**** 旁的核取方塊。 按一下 [完成] 。

    7.  您的範例應用程式已設定完成。 如果您測試應用程式 URL **https://webserv1.contoso.com/claimapp** ，它應該會將您重新導向至您的同盟伺服器。 同盟伺服器應該會顯示錯誤頁面，因為您尚未設定信賴憑證者信任。 換句話說，您 AD FS 不會保護此測試應用程式。

您現在必須使用 AD FS 來保護在 web 伺服器上執行的範例應用程式。 您可以透過在您的同盟伺服器 (ADFS1) 上新增信賴憑證者信任來執行此動作。 如需影片，請參閱 [Active Directory Federation Services How-To Video Series: Add a Relying Party Trust](https://channel9.msdn.com/Search?term=Active%20Directory%20Federation%20Services#pubDate=year&ch9Search)(Active Directory 同盟服務使用方法影片系列：新增信賴憑證者信任)。

### <a name="create-a-relying-party-trust-on-your-federation-server"></a><a name="BKMK_11"></a>在您的同盟伺服器建立信賴憑證者信任

1.  在您的同盟伺服器 (ADFS1) 上，在 [AD FS 管理] 主控台中****，瀏覽到 [信賴憑證者信任]****，然後按一下 [新增信賴憑證者信任]****。

2.  在 [選取資料來源]**** 頁面上，選取 [匯入發佈到線上或區域網路的信賴憑證者相關資料]****，輸入 **claimapp** 的中繼資料 URL，然後按 [下一步]****。 執行 FedUtil.exe 會建立中繼資料 .xml 檔案。 它位於 **https://webserv1.contoso.com/claimapp/federationmetadata/2007-06/federationmetadata.xml** 。

3.  在 [指定顯示名稱]**** 頁面上，為您的信賴憑證者信任指定**顯示名稱****claimapp**，然後按一下 [下一步]****。

4.  在 [立即設定多因素驗證?]**** 頁面上，選取 [我目前不想為此信賴憑證者信任指定多因素驗證設定]****，然後按一下 [下一步]****。

5.  在 [選擇發佈授權規則]**** 頁面上，選取 [允許所有使用者存取此信賴憑證者]****，然後按一下 [下一步]****。

6.  在 [準備新增信任]**** 頁面上，按一下 [下一步]****。

7.  在 [編輯宣告規則]**** 對話方塊中，按一下 [新增規則]****。

8.  在 [選擇規則類型]**** 頁面上，選取 [使用自訂規則傳送宣告]****，然後按一下 [下一步]****。

9. 在 [設定宣告規則]**** 頁面上的 [宣告規則名稱]**** 方塊中，輸入 **All Claims**。 在 [自訂規則]**** 方塊中，輸入下列宣告規則。

    ```
    c:[ ]
    => issue(claim = c);

    ```

10. 按一下 [完成]****，然後按一下 [確定]****。

## <a name="step-4-configure-the-client-computer-client1"></a><a name="BKMK_10"></a>步驟 4：設定用戶端電腦 (Client1)
設定另一部虛擬機器，並安裝 Windows 8.1。 此虛擬機器必須位於與其他機器一樣的虛擬網路中。 此機器不應該加入 Contoso 網域。

用戶端必須信任您在 [Step 2: Configure the federation server (ADFS1) with Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)中設定同盟伺服器 (ADFS1) 時所使用的 SSL 憑證。 它也必須能夠驗證憑證撤銷資訊。

您也必須設定 Microsoft 帳戶並使用它來登入 Client1。

## <a name="see-also"></a>另請參閱


- [Active Directory Federation Services How-To Video Series: Installing an AD FS Server Farm](https://channel9.msdn.com/Search?term=Active%20Directory%20Federation%20Services#pubDate=year&ch9Search)
- [Active Directory 同盟服務 How-to 影片系列：更新憑證](https://channel9.msdn.com/Search?term=Active%20Directory%20Federation%20Services#pubDate=year&ch9Search)
- [Active Directory Federation Services How-To Video Series: Add a Relying Party Trust](https://channel9.msdn.com/Search?term=Active%20Directory%20Federation%20Services#pubDate=year&ch9Search)
- [Active Directory Federation Services How-To Video Series: Enabling the Device Registration Service](https://channel9.msdn.com/)
- [Active Directory 同盟服務使用方法影片系列：安裝 Web 應用程式 Proxy](https://channel9.msdn.com/Search?term=Active%20Directory%20Federation%20Services#pubDate=year&ch9Search)
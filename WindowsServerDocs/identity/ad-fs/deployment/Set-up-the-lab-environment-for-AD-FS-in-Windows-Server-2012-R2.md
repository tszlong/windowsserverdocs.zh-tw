---
ms.assetid: 6b38480e-5b1c-49f0-9d46-8cf22f70f0d2
title: "在 Windows Server 2012 R2 AD FS 設定實驗室"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 049a1a1b0a419b0194edfe56b356a9f1e8b4b058
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="set-up-the-lab-environment-for-ad-fs-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 AD FS 設定實驗室

>適用於： Windows Server 2012 R2

本主題列出設定的測試環境，可以用來完成的逐步教學下列逐步解說指南中的步驟：

-   [逐步解說： IOS 裝置加入的工作地點](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

-   [Windows 裝置的逐步解說： 地點加入](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)


-   [逐步解說指南： 管理條件存取控制的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)

-   [逐步解說指南： 管理其他多因素驗證敏感的應用程式的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

> [!NOTE]
> 我們不建議在相同的電腦上安裝的網頁伺服器與聯盟伺服器。

若要設定此測試環境，請完成下列步驟：

1.  [步驟 1： 設定的網域控制站 (DC1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_1)

2.  [步驟 2： 使用裝置的登記服務設定聯盟伺服器 (ADFS1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)

3.  [步驟 3： 設定網頁伺服器 (WebServ1) 及範例宣告型應用程式](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_5)

4.  [步驟 4： 設定電腦 client (Client1)](../../ad-fs/deployment/../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_10)

## <a name="BKMK_1"></a>步驟 1： 設定的網域控制站 (DC1)
基於這項測試環境，您可以撥號您根 Active Directory domain **contoso.com** ，然後指定** pass@word1 **以系統管理員密碼。

-   安裝 AD DS 角色服務，並安裝 Active Directory Domain Services (AD DS)，在 Windows Server 2012 R2 網域控制站讓您的電腦。 這個動作升級您的網域控制站建立一部分的 AD DS 結構描述。 如需詳細資訊和逐步指示，請查看[https://technet.microsoft.com/library/hh472162.aspx](https://technet.microsoft.com/library/hh472162.aspx)。

### <a name="BKMK_2"></a>建立測試 Active Directory 帳號
您的網域控制站功能之後，您可以在這個網域中建立測試群組和測試使用者帳號，並加入該群組帳號帳號。 您可以使用這些帳號完成中參考此主題中前面逐步解說指南的逐步教學。

建立帳號：

-   使用者：**劉小龍 Hatley**的下列認證： 使用者名稱： **RobertH**及密碼：**P@ssword**

-   群組：**財經**

如需如何建立使用者和群組帳號 Active Directory（廣告）有關的資訊，請查看[https://technet.microsoft.com/library/cc783323%28v.aspx](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx)。

新增**劉小龍 Hatley**以帳號**財經**群組。 如需有關如何將使用者新增到群組 Active Directory 中資訊，請[https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx](https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx)。

### <a name="create-a-gmsa-account"></a>建立 GMSA 帳號
在 Active Directory 同盟 Services (AD FS) 安裝和設定，就需要群組管理服務 Account (GMSA) account。

##### <a name="to-create-a-gmsa-account"></a>若要建立 GMSA account

1.  Windows PowerShell 命令視窗中，輸入開放：

    ```
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    New-ADServiceAccount FsGmsa -DNSHostName adfs1.contoso.com -ServicePrincipalNames http/adfs1.contoso.com

    ```

## <a name="BKMK_4"></a>步驟 2： 使用裝置的登記服務設定聯盟伺服器 (ADFS1)
若要設定另一部一樣，安裝 Windows Server 2012 R2，並將它連接到網域**contoso.com**。之後您已經加入網域，設定電腦，然後繼續安裝並設定 AD FS 角色。

影片，請查看[Active Directory 同盟服務方法一系列影片： 安裝 AD FS 伺服器陣列](https://technet.microsoft.com/video/dn469436)。

### <a name="install-a-server-ssl-certificate"></a>安裝伺服器 SSL 憑證
您必須在電腦本機存放區 ADFS1 伺服器上安裝伺服器安全通訊端層 (SSL) 憑證。 憑證必須具備下列屬性：

-   主體名稱 (DATA-CN): adfs1.contoso.com

-   主旨替代名稱 」 (DNS): adfs1.contoso.com

-   主旨替代名稱 」 (DNS): enterpriseregistration.contoso.com

如需 SSL 憑證設定的詳細資訊，請查看[設定 SSL 日 TLS 網域中的網站上企業 CA 與](https://social.technet.microsoft.com/wiki/contents/articles/12485.configure-ssltls-on-a-web-site-in-the-domain-with-an-enterprise-ca.aspx)。

[Active Directory 同盟服務方法一系列的影片： 更新憑證](https://technet.microsoft.com/video/adfs-updating-certificates)。

### <a name="install-the-ad-fs-server-role"></a>安裝 AD FS 伺服器角色

##### <a name="to-install-the-federation-service-role-service"></a>若要安裝同盟服務的角色

1.  登入，使用系統管理員核對伺服器administrator@contoso.com。

2.  伺服器管理員 [開始]。 若要開始伺服器管理員中，按一下 [**伺服器管理員**windows **[開始]**畫面上，或按一下 [**伺服器管理員**在 Windows 工作列上的 Windows 桌面。 在**快速入門**索引標籤的**歡迎使用**] 磚上**儀表板**頁面上，按一下**新增角色與功能**。 或者，您可以按一下**新增角色與功能**在**管理**功能表。

3.  在**在您開始之前**頁面上，按一下 [**下**。

4.  上**選取 [安裝類型**頁面上，按一下 [**以角色為基礎，或為基礎的功能的安裝**，然後按一下 [**下一步**。

5.  在**選取目的伺服器**頁面上，按一下 [**伺服器集區中選取 [伺服器**，確認的目標電腦已選取，然後按**下一步**。

6.  在**選擇伺服器角色**頁面上，按一下 [ **Active Directory 同盟服務**，然後按一下 [**下一步**。

7.  在**選擇功能**頁面上，按一下 [**下**。

8.  在**Active Directory 同盟服務 (AD FS)**頁面上，按**下**。

9. 在確認此資訊後**確認安裝選項**頁面上，選取**必要時自動重新開機目的伺服器**核取方塊，並再按**安裝**。

10. 在**安裝進度**頁面，確認所有正確，安裝，然後按一下 [**關閉**。

### <a name="configure-the-federation-server"></a>將聯盟伺服器設定
下一個步驟是設定聯盟伺服器。

##### <a name="to-configure-the-federation-server"></a>若要將聯盟伺服器設定

1.  在伺服器管理員**儀表板**頁面上，按一下 [**通知**標幟，然後按一下 [**設定同盟服務，伺服器上**。

    **Active Directory 同盟服務設定精靈**開啟。

2.  在**歡迎使用**頁面上，選取**聯盟伺服器陣列中建立的第一個聯盟伺服器**，然後按一下 [**下一步**。

3.  在**連接到 AD DS**頁面上指定的 account 網域系統管理員權限的**contoso.com**這台電腦所加入的 Active Directory domain，然後按**下一步**。

4.  在**指定服務屬性**頁面上，執行下列命令，，然後按**下**:

    -   匯入之前取得 SSL 憑證。 這是憑證所需的服務驗證憑證。 瀏覽至您的 SSL 憑證的位置。

    -   若要提供您同盟服務的名稱，輸入**adfs1.contoso.com**。這個的值為您提供您退出 SSL 憑證在 Active Directory 憑證 Services (AD CS) 時的相同的值。

    -   若要提供您同盟服務的顯示名稱，輸入**以 Contoso Corporation**。

5.  在**指定服務 Account**頁面上，選取**現有的網域帳號使用者或群組管理服務 Account**，然後指定 GMSA account **fsgmsa**您建立您的網域控制站在建立時。

6.  在**指定設定資料庫**頁面上，選取**建立在使用 Windows 內部資料庫此伺服器上的資料庫**，然後按一下 [**下一步**。

7.  在**評論選項**頁面，確認您的設定選項，然後按一下 [**下**。

8.  在**必要條件檢查**頁面上，確認所有必要條件檢查已成功完成，然後按**設定**。

9. 在**結果**頁面上，檢視結果，請檢查是否已成功完成設定，然後按一下**完成同盟服務部署所需的下一個步驟**。

### <a name="configure-device-registration-service"></a>設定裝置登記服務
下一個步驟是設定裝置登記服務 ADFS1 伺服器上。 影片，請查看[Active Directory 同盟服務方法一系列影片： 讓裝置登記服務](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service)。

##### <a name="to-configure-device-registration-service-for-windows-server-2012-rtm"></a>若要設定裝置登記服務適用於 Windows Server 2012 RTM

1.  > [!IMPORTANT]
    > **Windows Server 2012 R2 RTM 建置適用於下列步驟。**

    Windows PowerShell 命令視窗中，輸入開放：

    ```
    Initialize-ADDeviceRegistration
    ```

    當系統提示您服務的帳號時，輸入**contoso\fsgmsa$**。

    現在，執行的 Windows PowerShell cmdlet。

    ```
    Enable-AdfsDeviceRegistration
    ```

2.  ADFS1 伺服器上的**AD FS 管理**主機、 瀏覽至**驗證原則**。 選取 [**編輯全球主要驗證**。 選取核取方塊接下來**讓裝置驗證**，然後按一下 [ **[確定]**。

### <a name="add-host-a-and-alias-cname-resource-records-to-dns"></a>主機 (A) 及別名 (CNAME) 資源記錄加入 DNS
在 DC1，您必須建立的網域名稱系統 」 (DNS) 下列記錄裝置登記服務。

|項目|輸入|地址|
|---------|--------|-----------|
|adfs1|主機 (A)|AD FS 伺服器的 IP 位址|
|enterpriseregistration|別名 (CNAME)|adfs1.contoso.com|

若要新增公司名稱 dns 伺服器聯盟和裝置登記服務主機 (A) 資源記錄，您可以使用下列程序。

系統管理員群組或相當於成員資格已完成此程序的最低需求。 檢視詳細資料使用適當帳號和超連結「https://go.microsoft.com/fwlink/?LinkId=83477「本機和網域預設群組 (https://go.microsoft.com/fwlink/p/?LinkId=83477) 群組成員資格。

##### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>加入主機 (A) 及別名 (CNAME) 資源記錄 DNS 伺服器聯盟

1.  在 DC1，從伺服器管理員中，在**工具**功能表上，按**DNS**打開 DNS 嵌入式管理單元。

2.  主控台依序展開 DC1、**正向對應區域**上, 按一下滑鼠右鍵**contoso.com**，然後按一下 [**新主機 （或 AAAA）**。

3.  在**的名稱，**輸入您想要使用 AD FS 發電廠您的名稱。 本節中，輸入**adfs1**。

4.  在**的 IP 位址**，輸入 ADFS1 伺服器的 IP 位址。 按一下**新增主機**。

5.  以滑鼠右鍵按一下**contoso.com**，然後按**新別名 (CNAME)**。

6.  在**新資源記錄**對話方塊中，輸入**enterpriseregistration**中**別名**方塊。

7.  在完全完整網域名稱 (FQDN) 的目標主機方塊中，輸入**adfs1.contoso.com**，然後按**[確定]**。

    > [!IMPORTANT]
    > 在現實世界的部署，如果您的公司有多個使用者主體名稱 (UPN) 尾碼，您必須建立多個 CNAME 記錄 dns 這些 UPN 尾碼個。

## <a name="BKMK_5"></a>步驟 3： 設定網頁伺服器 (WebServ1) 及範例宣告型應用程式
設定一樣 (WebServ1) 來安裝 Windows Server 2012 R2 的作業系統，並將它連接到網域**contoso.com**。它已經加入網域之後，您就可以設定網頁伺服器角色加以安裝。

若要完成稍早本主題中所參照的逐步教學，您必須聯盟伺服器 (ADFS1) 受保護的範例應用程式。

您可以下載身分基本知識的 Windows SDK ([https://www.microsoft.com/download/details.aspx?id=4451](https://www.microsoft.com/download/details.aspx?id=4451)，其中包含一個範例宣告型應用程式。

您必須先完成下列步驟來設定此範例宣告型應用程式與 web 伺服器。

> [!NOTE]
> 執行 Windows Server 2012 R2 作業系統的網頁伺服器經過這些步驟。

1.  [安裝 Windows 的身分基礎與 Web 伺服器角色](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_15)

2.  [安裝 Windows SDK 的身分基礎](../../ad-fs/deployment/../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_13)

3.  [在設定簡單宣告應用程式](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_9)

4.  [聯盟伺服器上建立信賴廠商信任](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_11)

### <a name="BKMK_15"></a>安裝 Windows 的身分基礎與 Web 伺服器角色

1.  > [!NOTE]
    > 您必須在 Windows Server 2012 R2 的安裝媒體來存取。

    登入 WebServ1 使用** administrator@contoso.com **的密碼和** pass@word1 **。

2.  從伺服器管理員中，在**快速入門**索引標籤的**歡迎使用**] 磚上**儀表板**頁面上，按一下**新增角色與功能**。 或者，您可以按一下**新增角色與功能**在**管理**功能表。

3.  在**在您開始之前**頁面上，按一下 [**下**。

4.  上**選取 [安裝類型**頁面上，按一下 [**以角色為基礎，或為基礎的功能的安裝**，然後按一下 [**下一步**。

5.  在**選取目的伺服器**頁面上，按一下 [**伺服器集區中選取 [伺服器**，確認的目標電腦已選取，然後按**下一步**。

6.  在**選取伺服器角色**頁面上，選取核取方塊接下來**網頁伺服器 (IIS)**，按一下 [**新增功能**，，然後按一下**下一步**。

7.  在**選取功能**頁面上，選取**Windows 身分基本知識 3.5**，然後按一下 [**下一步**。

8.  在**網頁伺服器角色 (IIS)**頁面上，按**下**。

9. 在**選擇角色服務**頁面上，選取 [展開**應用程式開發**。 選取 [ **ASP.NET 3.5**，按一下 [**新增功能**，然後按一下 [**下一步**。

10. 在**確認安裝選項**頁面上，按一下 [**指定替代來源路徑**。 Windows Server 2012 R2 的安裝媒體位於 Sxs directory 輸入的路徑。 例如，D:\Sources\Sxs。 按一下**[確定]**，然後按**安裝**。

### <a name="BKMK_13"></a>安裝 Windows SDK 的身分基礎

1.  執行 WindowsIdentityFoundation-SDK-3.5.msi 安裝 Windows 的身分基本知識 SDK 3.5 (https://www.microsoft.com/download/details.aspx?id=4451)。 請選擇預設的選項。

### <a name="BKMK_9"></a>在設定簡單宣告應用程式

1.  憑證存放區的電腦安裝有效 SSL 憑證。 憑證應包含您的網頁伺服器的名稱**webserv1.contoso.com**。

2.  複製到 C:\Program 檔案 (x86) \Windows Identity Foundation SDK\v3.5\Samples\Quick Start\Web Application\PassiveRedirectBasedClaimsAwareWebApp 以 C:\Inetpub\Claimapp。

3.  編輯**Default.aspx.cs** ，讓任何宣告篩選發生檔案。 執行此步驟以確保的範例應用程式顯示所有宣告所發行的聯盟伺服器。 執行下列動作：

    1.  開放**Default.aspx.cs**在文字編輯器中。

    2.  搜尋檔案的第二個`ExpectedClaims`。

    3.  查看整個意見`IF`聲明，並其括號。 表示意見，輸入 「 日日 」 （不含報價） 一行的開頭。

    4.  您`FOREACH`隱私權聲明應該看起來像此程式碼範例。

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

    5.  儲存，並關閉**Default.aspx.cs**。

    6.  開放**web.config**在文字編輯器中。

    7.  移除整個`<microsoft.identityModel>`區段。 移除所有項目從`including <microsoft.identityModel>`最多及包括`</microsoft.identityModel>`。

    8.  儲存，並關閉**web.config**。

4.  **設定 IIS 管理員**

    1.  開放**資訊網際網路服務 (IIS) 管理員**。

    2.  移至**應用程式集區**，以滑鼠右鍵按一下**DefaultAppPool**以選取 [**進階設定]**。 設定**載入的使用者設定檔**以**為 True**，然後按一下 [ **[確定]**。

    3.  以滑鼠右鍵按一下**DefaultAppPool**選取**設定基本**。 變更**.NET CLR 版本**以**.NET CLR 版本 v2.0.50727**。

    4.  以滑鼠右鍵按一下**預設的網站**選取**編輯繫結**。

    5.  新增**HTTPS**繫結至連接埠**443**安裝 SSL 憑證。

    6.  以滑鼠右鍵按一下**預設的網站**選取**新增應用程式**。

    7.  為別名**claimapp**的實體路徑和**c:\inetpub\claimapp**。

5.  若要設定**claimapp**若要搭配您的聯盟伺服器，執行下列動作：

    1.  執行 FedUtil.exe 位於**C:\Program 檔案 (x86) \Windows Identity Foundation SDK\v3.5**。

    2.  將應用程式設定位置設定**C:\inetput\claimapp\web.config**設為您的網站 URL URI 應用程式和**https://webserv1.contoso.com /claimapp/**。 按一下**下一步**。

    3.  選取 [**使用現有 STS** ，然後瀏覽至您的 AD FS 伺服器中繼資料 URL **https://adfs1.contoso.com/federationmetadata/2007-06/federationmetadata.xml**。 按一下**下一步**。

    4.  選取 [**停用的憑證鏈結驗證**，然後按一下 [**下**。

    5.  選取 [**未加密**，然後按一下 [**下**。 在**提供宣告**頁面上，按**下**。

    6.  選取核取方塊旁邊**排程工作執行日常 WS 同盟中繼資料更新**。 按一下**完成**。

    7.  範例應用程式現在設定。 如果您在測試應用程式 URL **https://webserv1.contoso.com/claimapp**，它應該會重新導向至您的聯盟伺服器。 因為您未尚未設定信賴的派對信任聯盟伺服器應該會顯示錯誤頁面。 亦即，您不安全，AD FS 此測試應用程式。

現在，您必須保護您的網頁伺服器使用 AD FS 上執行的範例應用程式。 您可以藉由信賴的派對信任新增聯盟伺服器 (ADFS1) 上執行此動作。 影片，請查看[Active Directory 同盟服務方法一系列影片： 新增可以方信任](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust)。

### <a name="BKMK_11"></a>聯盟伺服器上建立信賴廠商信任

1.  您聯盟伺服器上 (ADFS1)，在**AD FS 管理主控台**，瀏覽至**可以信任派對**，，然後按一下 [**新增可以方信任**。

2.  在**選取資料來源**頁面上，選取 [**匯入信賴有關的資料發行 online 或本機網路上**，輸入中繼資料 URL **claimapp**，，然後按一下**下一步**。 執行 FedUtil.exe 建立中繼資料.xml 檔案。 這是位於**https://webserv1.contoso.com/claimapp/federationmetadata/2007-06/federationmetadata.xml**。

3.  在**指定顯示名稱**頁面上指定**顯示名稱**您信賴的派對信任的**claimapp**，然後按一下 [**下一步**。

4.  在**設定現在多因素驗證嗎？**頁面上，選取**我不想指定多因素驗證設定信賴信任派對這次**，然後按一下**下一步**。

5.  在**選擇發行授權規則**頁面上，選取**允許所有使用者存取此信賴**，然後按一下 [**下一步**。

6.  在**準備好新增信任**頁面上，按一下 [**下**。

7.  在**編輯理賠要求規則**對話方塊中，按**[新增規則**。

8.  在**選擇規則類型**頁面上，選取**傳送主張使用自訂規則**，然後按一下 [**下一步**。

9. 在**設定理賠要求規則**頁面上，在**理賠要求規則名稱**方塊中，輸入**所有宣告**。 在**自訂規則**方塊中，輸入下列理賠要求規則。

    ```
    c:[ ]
    => issue(claim = c);

    ```

10. 按一下**完成**，然後按**[確定]**。

## <a name="BKMK_10"></a>步驟 4： 設定電腦 client (Client1)
設定其他一樣，安裝 Windows 8.1。 這個一樣必須是相同的 virtual 網路的其他電腦上。 這台電腦應該不會以 Contoso 網域加入。

Client 必須信任 SSL 憑證聯盟伺服器 (ADFS1)，您在設定使用[步驟 2： 設定聯盟伺服器 (ADFS1) 裝置登記服務的](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)。 它也必須驗證憑證的憑證撤銷資訊。

您還必須設定並使用 Microsoft account Client1 登入。

## <a name="see-also"></a>也了


- [Active Directory 同盟服務方法一系列影片： 安裝 AD FS 伺服器農場](https://technet.microsoft.com/video/dn469436)
- [更新憑證 active Directory 同盟服務一系列影片方法：](https://technet.microsoft.com/video/adfs-updating-certificates)
- [Active Directory 同盟服務方法一系列影片： 加入派對信任依賴](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust)
- [Active Directory 同盟服務方法一系列的影片： 讓裝置登記服務](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service)
- [Active Directory 同盟服務方法一系列的影片： 安裝應用程式網路 Proxy](https://technet.microsoft.com/video/dn469438)




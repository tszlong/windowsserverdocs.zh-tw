---
ms.assetid: 5f733510-c96e-4d3a-85d2-4407de95926e
title: 使用 AD FS 預先驗證發佈應用程式
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: web-app-proxy
ms.openlocfilehash: ca4d8661f8f0252334bdecbde85603d8af5e2d2a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446817"
---
# <a name="publishing-applications-using-ad-fs-preauthentication"></a>使用 AD FS 預先驗證發佈應用程式

>適用於：Windows Server 2016

**此內容是關於 Web 應用程式 Proxy 的內部部署版本的項目。若要在雲端上啟用安全存取內部部署應用程式，請參閱[Azure AD Application Proxy 內容](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/)。**  
  
本主題描述如何透過使用 Active Directory Federation Services (AD FS) 預先驗證的 Web 應用程式 Proxy 發佈應用程式。  
  
您可以使用 AD FS 預先驗證對其進行發佈的應用程式的所有類型，您必須新增信賴憑證者信任 Federation service AD FS。  
  
一般的 AD FS 預先驗證流程如下所示：  
  
> [!NOTE]  
> 此驗證流程不適用於使用 Microsoft Store 應用程式的用戶端。  
  
1.  用戶端裝置嘗試存取特定資源 URL; 上的已發佈的 web 應用程式比方說 https://app1.contoso.com/。  
  
    資源 URL 是 Web 應用程式 Proxy 接聽傳入 HTTPS 要求的公用位址。  
  
    如果已啟用 HTTP 至 HTTPS 重新導向，Web 應用程式 Proxy 也會接聽傳入的 HTTP 要求。  
  
2.  Web 應用程式 Proxy 會將 HTTPS 要求重新導向到 AD FS 伺服器，以 URL 編碼參數，包括資源 URL 和 appRealm （信賴憑證者的合作對象識別項）。  
  
    使用者驗證是使用 AD FS 伺服器所需的驗證方法例如，使用者名稱和密碼、 一次性密碼等等的雙因素驗證。  
  
3.  使用者經過驗證之後，AD FS 伺服器發出安全性權杖，'edge 權杖'，其中包含下列資訊和重新導向回 Web 應用程式 Proxy 伺服器的 HTTPS 要求：  
  
    -   使用者嘗試存取的資源識別碼。  
  
    -   使用者的身分識別，做為使用者主體名稱 (UPN)。  
  
    -   授與核准存取的到期日期；也就是說，只會授與使用者一段限定時間的存取權，過了這個時間，就會要求使用者重新驗證。  
  
    -   edge 權杖資訊的簽章  
  
4.  Web 應用程式 Proxy 從包含 edge 權杖的 AD FS 伺服器接收重新導向的 HTTPS 要求，然後驗證並使用權杖，如下所示：  
  
    -   驗證 edge 權杖簽章是從 federation service 中 Web 應用程式 Proxy 組態設定。  
  
    -   驗證發行的是正確的應用程式權杖。  
  
    -   驗證權杖尚未過期。  
  
    -   視需要使用使用者身分識別，例如，如果後端伺服器設定要使用整合式 Windows 驗證，就要取得 Kerberos 票證。  
  
5.  如果 edge 權杖是有效的 Web 應用程式 Proxy 會轉送使用 HTTP 或 HTTPS 的已發佈的 web 應用程式的 HTTPS 要求。  
  
6.  用戶端現在可以存取發行的 Web 應用程式；不過，已發行的應用程式可能會設定要求使用者執行其他驗證。 例如，如果發行的 Web 應用程式是 SharePoint 網站，而且不需要其他驗證，則使用者會在瀏覽器中看到 SharePoint 網站。  
  
7.  Web 應用程式 Proxy 會將 cookie 儲存在用戶端裝置上。 Cookie 是 Web 應用程式 Proxy，用來識別此工作階段具有已經過預先驗證，而且需要任何進一步的預先驗證。  
  
> [!IMPORTANT]  
> 設定外部 URL 和後端伺服器 URL 時，確定您包含完整網域名稱 (FQDN)，而不是 IP 位址。  
  
> [!NOTE]  
> 本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_1.1"></a>發行宣告式應用程式的 Web 瀏覽器用戶端  
若要發行使用宣告進行驗證的應用程式，您必須將應用程式的信賴憑證者信任新增至 Federation Service。  
  
發行宣告式應用程式以及從瀏覽器存取應用程式的時候，一般驗證流程如下：  
  
1.  用戶端嘗試存取使用網頁瀏覽器; 的宣告式應用程式比方說， https://appserver.contoso.com/claimapp/。  
  
2.  網頁瀏覽器將要求重新導向到 AD FS 伺服器的 Web 應用程式 Proxy 伺服器來傳送 HTTPS 要求。  
  
3.  在 AD FS 伺服器會驗證使用者和裝置，並將要求重新導向回 Web 應用程式 Proxy。 要求現在包含 edge 權杖。 在 AD FS 伺服器會將單一登入 (SSO) cookie 新增至要求中，因為使用者已經執行 AD FS 伺服器的驗證。  
  
4.  Web 應用程式 Proxy 會驗證權杖、 新增自己的 cookie，並將要求轉寄到後端伺服器。  
  
5.  後端伺服器的要求重新導向至 AD FS 伺服器，以取得應用程式的安全性權杖。  
  
6.  已重新導向到 AD FS 伺服器後端伺服器。 要求現在包含應用程式權杖和 SSO Cookie。 使用者已獲得應用程式的存取權，不需要輸入使用者名稱或密碼。  
  
這個程序描述如何發行網頁瀏覽器用戶端存取的宣告式應用程式 (如 SharePoint 網站)。 在開始之前，請確定您已完成下列各項：  
  
-   在 AD FS 管理主控台中建立應用程式的信賴憑證者信任。  
  
-   驗證 Web 應用程式 Proxy 伺服器上的憑證是適用於您想要發佈應用程式。  
  

  
#### <a name="to-publish-a-claims-based-application"></a>發行宣告式應用程式  
  
1.  在 Web 應用程式 Proxy 伺服器上，在遠端存取管理主控台中，在**瀏覽**窗格中，按一下**Web 應用程式 Proxy**，然後在**工作** 窗格中，按一下**發佈**。  
  
2.  在 [發行新應用程式精靈]  的 [歡迎]  頁面上，按 [下一步]  。  
  
3.  在 [**預先驗證**頁面上，按一下**Active Directory Federation Services (AD FS)** ，然後按一下**下一步]** 。  
  
4.  在 [支援的用戶端]  頁面上，選取 [Web 和 MSOFBA]  ，然後按 [下一步]  。  
  
5.  在 [信賴憑證者]  頁面上，從信賴憑證者的清單中選取想要發行之應用程式的信賴憑證者，然後按 [下一步]  。  
  
6.  在 [發行設定]  頁面上，執行下列動作，然後按 [下一步]  ：  
  
    -   在 [名稱]  方塊中，輸入易記的應用程式名稱。  
  
        這個名稱僅用於 [遠端存取管理] 主控台中已發行應用程式的清單中。  
  
    -   在 [外部 URL]  方塊中，輸入這個應用程式的外部 URL，例如 https://sp.contoso.com/app1/。  
  
    -   在 [外部憑證]  清單中，選取主體涵蓋外部 URL 的憑證。  
  
    -   在 [後端伺服器 URL]  方塊中，輸入後端伺服器的 URL。 請注意，當您輸入外部 URL 應該只在後端伺服器 URL 不同; 如果變更會自動輸入此值比方說， https://sp/app1/。  
  
        > [!NOTE]  
        > Web 應用程式 Proxy 可以轉譯 Url 中的主機名稱，但是不能轉譯路徑名稱。 因此，您可以輸入不同的主機名稱，但是必須輸入相同的路徑名稱。 例如，您可以輸入外部 URL https://apps.contoso.com/app1/以及後端伺服器 URL 的 https://app-server/app1/。 不過，您無法在此輸入的外部 URL https://apps.contoso.com/app1/以及後端伺服器 URL 的 https://apps.contoso.com/internal-app1/。  
  
7.  在 [確認]  頁面上，檢查設定，然後按一下 [發行]  。 您可以複製 PowerShell 命令來設定其他的已發行應用程式。  
  
8.  在 [結果]  頁面上，確認已成功發行應用程式，然後按一下 [關閉]  。  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerURL 'https://sp.contoso.com/app1/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://sp.contoso.com/app1/'  
    -Name 'SP'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'SP_Relying_Party'  
```  
  
## <a name="BKMK_1.2"></a>發佈的 Web 瀏覽器用戶端的整合式 Windows 驗證式應用程式  
Web 應用程式 Proxy 可以用來發行使用整合式 Windows 驗證，應用程式亦即，Web 應用程式 Proxy 執行視需要預先驗證，並可為使用整合式 Windows 驗證發佈應用程式執行 SSO。 若要發行使用整合式 Windows 驗證的應用程式，您必須將應用程式的非宣告感知信賴憑證者信任新增至 Federation Service。  
  
若要允許 Web 應用程式 Proxy 來執行單一登入 (SSO)，以及委派執行認證使用 Kerberos 限制委派，Web Application Proxy 伺服器必須加入網域。 請參閱[規劃 Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD)。  
  
若要允許使用者存取使用整合式 Windows 驗證的應用程式，Web Application Proxy 伺服器必須能夠提供讓使用者發佈的應用程式的委派。 您可以在網域控制站上為任何應用程式執行這項操作。 您也可以執行這個後端伺服器上執行 Windows Server 2012 R2 或 Windows Server 2012 上。 如需詳細資訊，請參閱 [Kerberos 驗證的新功能](https://technet.microsoft.com/library/hh831747.aspx)。  
  
如何設定 Web 應用程式 Proxy 發佈應用程式使用整合式 Windows 驗證的逐步解說，請參閱[站台設定為使用整合式 Windows 驗證](https://technet.microsoft.com/library/dn280943.aspx#BKMK_3)。  
  
使用後端伺服器的整合式 Windows 驗證時，Web 應用程式 Proxy 與已發行的應用程式之間的驗證不是宣告為基礎，而是會使用 Kerberos 限制委派來驗證應用程式的使用者。 一般流程如下所述：  
  
1.  用戶端嘗試存取非宣告式應用程式使用 web 瀏覽器;比方說， https://appserver.contoso.com/nonclaimapp/。  
  
2.  網頁瀏覽器將要求重新導向到 AD FS 伺服器的 Web 應用程式 Proxy 伺服器來傳送 HTTPS 要求。  
  
3.  在 AD FS 伺服器會驗證使用者，並將要求重新導向回 Web 應用程式 Proxy。 要求現在包含 edge 權杖。  
  
4.  Web 應用程式 Proxy 會驗證權杖。  
  
5.  如果權杖有效，Web 應用程式 Proxy 會從網域控制站取得 Kerberos 票證代表使用者中。  
  
6.  Web 應用程式 Proxy 將要求中的 Kerberos 票證，做為 Simple and Protected GSS-API Negotiation Mechanism (SPNEGO) 權杖的一部分，並將要求轉寄到後端伺服器。 要求包含 Kerberos 票證，因此，使用者取得應用程式的存取權，且不需要進一步驗證。  
  
這個程序描述如何發行網頁瀏覽器用戶端存取的整合式 Windows 驗證應用程式 (如 Outlook Web 應用程式)。 在開始之前，請確定您已完成下列各項：  
  
-   在 AD FS 管理主控台中建立的非宣告感知信賴憑證者信任的應用程式。  
  
-   設定後端伺服器以便在網域控制站上支援 Kerberos 限制委派，或使用 Set-ADUser Cmdlet 搭配 -PrincipalsAllowedToDelegateToAccount 參數。 請注意，是否後端伺服器執行 Windows Server 2012 R2 或 Windows Server 2012 上，您也可以執行此 PowerShell 命令後端伺服器上。  
  
-   確保 Web 應用程式 Proxy 伺服器已委派給服務主體名稱的後端伺服器。  
  
-   驗證 Web 應用程式 Proxy 伺服器上的憑證是適用於您想要發佈應用程式。  
  
 
  
#### <a name="to-publish-a-non-claims-based-application"></a>發行非宣告式應用程式  
  
1.  在 Web 應用程式 Proxy 伺服器上，在遠端存取管理主控台中，在**瀏覽**窗格中，按一下**Web 應用程式 Proxy**，然後在**工作** 窗格中，按一下**發佈**。  
  
2.  在 [發行新應用程式精靈]  的 [歡迎]  頁面上，按 [下一步]  。  
  
3.  在 [**預先驗證**頁面上，按一下**Active Directory Federation Services (AD FS)** ，然後按一下**下一步]** 。  
  
4.  在 [支援的用戶端]  頁面上，選取 [Web 和 MSOFBA]  ，然後按 [下一步]  。  
  
5.  在 [信賴憑證者]  頁面上，從信賴憑證者的清單中選取想要發行之應用程式的信賴憑證者，然後按 [下一步]  。  
  
6.  在 [發行設定]  頁面上，執行下列動作，然後按 [下一步]  ：  
  
    -   在 [名稱]  方塊中，輸入易記的應用程式名稱。  
  
        這個名稱僅用於 [遠端存取管理] 主控台中已發行應用程式的清單中。  
  
    -   在 [外部 URL]  方塊中，輸入這個應用程式的外部 URL，例如 https://owa.contoso.com/。  
  
    -   在 [外部憑證]  清單中，選取主體涵蓋外部 URL 的憑證。  
  
    -   在 [後端伺服器 URL]  方塊中，輸入後端伺服器的 URL。 請注意，當您輸入外部 URL 應該只在後端伺服器 URL 不同; 如果變更會自動輸入此值比方說， https://owa/。  
  
        > [!NOTE]  
        > Web 應用程式 Proxy 可以轉譯 Url 中的主機名稱，但是不能轉譯路徑名稱。 因此，您可以輸入不同的主機名稱，但是必須輸入相同的路徑名稱。 例如，您可以輸入外部 URL https://apps.contoso.com/app1/以及後端伺服器 URL 的 https://app-server/app1/。 不過，您無法在此輸入的外部 URL https://apps.contoso.com/app1/以及後端伺服器 URL 的 https://apps.contoso.com/internal-app1/。  
  
    -   在 [後端伺服器 SPN]  方塊中，輸入後端伺服器的服務主體名稱，例如，HTTP/owa.contoso.com。  
  
7.  在 [確認]  頁面上，檢查設定，然後按一下 [發行]  。 您可以複製 PowerShell 命令來設定其他的已發行應用程式。  
  
8.  在 [結果]  頁面上，確認已成功發行應用程式，然後按一下 [關閉]  。  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerAuthenticationSpn 'HTTP/owa.contoso.com'  
    -BackendServerURL 'https://owa.contoso.com/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://owa.contoso.com/'  
    -Name 'OWA'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'Non-Claims_Relying_Party'  
```  
  
## <a name="BKMK_1.3"></a>發行使用 MS OFBA 的應用程式  
Web 應用程式 Proxy 支援從 Microsoft Office 用戶端，例如 Microsoft Word 存取該存取文件和資料後端伺服器上。 這些應用程式與標準瀏覽器之間唯一的差別是重新導向到 STS 是不是透過一般 HTTP 重新導向，但使用特殊的 MS OFBA 標頭中所指定： [ https://msdn.microsoft.com/library/dd773463(v=office.12).aspx ](https://msdn.microsoft.com/library/dd773463(v=office.12).aspx)。 後端應用程式可以是宣告或 IWA。   
若要發行使用 MS OFBA 的用戶端應用程式，您必須新增至 Federation Service 的應用程式的信賴憑證者信任。 取決於應用程式，您可以使用宣告式驗證或整合式 Windows 驗證。 所以，您必須根據應用程式來新增相關的信賴憑證者信任。  
  
若要允許 Web 應用程式 Proxy 來執行單一登入 (SSO)，以及委派執行認證使用 Kerberos 限制委派，Web Application Proxy 伺服器必須加入網域。 請參閱[規劃 Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD)。  
  
如果應用程式使用宣告型驗證，則沒有任何額外的規劃步驟。 如果應用程式使用整合式 Windows 驗證，請參閱[發佈的 Web 瀏覽器用戶端的整合式 Windows 驗證式應用程式](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2)。  
  
以下說明使用 MS OFBA 通訊協定使用宣告式驗證的用戶端的驗證流程。 這個案例的驗證可以在 URL 或內文中使用應用程式權杖。  
  
1.  使用者正在使用 Office 程式，他從 [最近的文件]  清單中，開啟 SharePoint 網站上的一個檔案。  
  
2.  Office 程式會顯示一個包含瀏覽控制項的視窗，要求使用者輸入認證。  
  
    > [!NOTE]  
    > 在某些情況下視窗可能不會出現，因為用戶端已經過驗證。  
  
3.  Web 應用程式 Proxy 將要求重新導向到 AD FS 伺服器，以執行驗證。  
  
4.  在 AD FS 伺服器的要求重新導向回 Web 應用程式 Proxy。 要求現在包含 edge 權杖。  
  
5.  在 AD FS 伺服器會將單一登入 (SSO) cookie 新增至要求中，因為使用者已經執行 AD FS 伺服器的驗證。  
  
6.  Web 應用程式 Proxy 會驗證權杖，並將要求轉寄到後端伺服器。  
  
7.  後端伺服器的要求重新導向至 AD FS 伺服器，以取得應用程式的安全性權杖。  
  
8.  要求已重新導向到後端伺服器。 要求現在包含應用程式權杖和 SSO Cookie。 使用者已獲得 SharePoint 網站的存取權，不需要輸入使用者名稱或密碼就能檢視檔案。  
  
發行使用 MS OFBA 的應用程式的步驟完全相同的宣告式應用程式或非宣告式應用程式的步驟。 對於宣告為基礎的應用程式，請參閱[發行宣告式應用程式的 Web 瀏覽器用戶端](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.1)，對於非宣告式應用程式，請參閱[發行適用於 Web 的整合式 Windows 驗證式應用程式瀏覽器用戶端](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2)。 Web 應用程式 Proxy 會自動偵測用戶端，並將驗證所需的使用者。  
  
## <a name="publish-an-application-that-uses-http-basic"></a>發行使用 HTTP 基本的應用程式  

HTTP 基本驗證是許多通訊協定，用來連接豐富型用戶端，包括智慧型手機，使用您的 Exchange 信箱的授權通訊協定。 如需有關 HTTP 基本的詳細資訊，請參閱[RFC 2617](https://www.ietf.org/rfc/rfc2617.txt)。 與 AD FS 使用重新導向，傳統上互動的 web 應用程式 Proxy大部分的豐富型用戶端不支援 cookie 或狀態管理。 如此一來在 Web 應用程式 Proxy 可讓 HTTP 應用程式接收非宣告信賴憑證者信任 Federation Service 應用程式。 請參閱[規劃 Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD)。  
  
下面，而在此圖說明使用 HTTP 基本用戶端的驗證流程：  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/WebApplicationProxy_httpBasicflow.png)  
  
1.  使用者嘗試存取已發佈的 web 應用程式的電話用戶端。  
  
2.  應用程式會將 HTTPS 要求傳送至 Web 應用程式 Proxy 所發佈的 URL。  
  
3.  如果要求不包含認證、 Web 應用程式 Proxy HTTP 401 回應傳回給包含驗證的 AD FS 伺服器的 URL 的應用程式。  
  
4.  使用者傳送應用程式的 HTTPS 要求重新設定為基本和使用者名稱和 Base 64 的授權加密 www 中使用者的密碼-驗證要求標頭。  
  
5.  因為裝置無法重新導向至 AD FS 中，Web 應用程式 Proxy 傳送驗證要求至 AD FS 使用的認證，其包括使用者名稱和密碼。 代表裝置取得權杖。  
  
6.  若要傳送至 AD FS 的要求數目降到最低，Web Application Proxy 會驗證語彙基元有效的情況下有使用快取的權杖，如後續的用戶端要求。 Web 應用程式 Proxy 會定期清除快取。 您可以檢視使用效能計數器的快取大小。  
  
7.  如果權杖有效，Web 應用程式 Proxy 將要求轉寄到後端伺服器，並授與使用者存取已發佈的 web 應用程式。  
  
下列程序說明如何將 HTTP 基本應用程式的發行。  
  
#### <a name="to-publish-an-http-basic-application"></a>HTTP 基本應用程式發行  
  
1.  在 Web 應用程式 Proxy 伺服器上，在遠端存取管理主控台中，在**瀏覽**窗格中，按一下**Web 應用程式 Proxy**，然後在**工作** 窗格中，按一下**發佈**。  
  
2.  在 [發行新應用程式精靈]  的 [歡迎]  頁面上，按 [下一步]  。  
  
3.  在 [**預先驗證**頁面上，按一下**Active Directory Federation Services (AD FS)** ，然後按一下**下一步]** 。  
  
4.  在 **支援的用戶端**頁面上，選取**HTTP 基本**，然後按一下 **下一步**。  
  
    如果您想要啟用對 Exchange 的存取，只能從已加入工作場所的裝置，請選取**啟用存取，只會針對工作場所加入裝置** 方塊中。 如需詳細資訊，請參閱[從任何裝置加入工作地點網路提供 SSO 和無縫式第二個因素 Authentication Across Company Applications](https://technet.microsoft.com/library/dn280945.aspx)。  
  
5.  在 [信賴憑證者]  頁面上，從信賴憑證者的清單中選取想要發行之應用程式的信賴憑證者，然後按 [下一步]  。 請注意，此清單僅包含上宣告信賴憑證者的合作對象。  
  
6.  在 [發行設定]  頁面上，執行下列動作，然後按 [下一步]  ：  
  
    -   在 [名稱]  方塊中，輸入易記的應用程式名稱。  
  
        這個名稱僅用於 [遠端存取管理] 主控台中已發行應用程式的清單中。  
  
    -   在 **外部 URL**方塊中，輸入這個應用程式的外部 URL，例如 mail.contoso.com  
  
    -   在 [外部憑證]  清單中，選取主體涵蓋外部 URL 的憑證。  
  
    -   在 [後端伺服器 URL]  方塊中，輸入後端伺服器的 URL。 請注意，當您輸入外部 URL 應該只在後端伺服器 URL 不同; 如果變更會自動輸入此值例如，mail.contoso.com。  
  
7.  在 [確認]  頁面上，檢查設定，然後按一下 [發行]  。 您可以複製 PowerShell 命令來設定其他的已發行應用程式。  
  
8.  在 [結果]  頁面上，確認已成功發行應用程式，然後按一下 [關閉]  。  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
此 Windows PowerShell 指令碼可讓預先驗證適用於所有裝置，不只是已加入工作場所的裝置。  
  
```  
Add-WebApplicationProxyApplication  
     -BackendServerUrl 'https://mail.contoso.com'   
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'   
     -ExternalUrl 'https://mail.contoso.com'   
     -Name 'Exchange'   
     -ExternalPreAuthentication ADFSforRichClients  
     -ADFSRelyingPartyName 'EAS_Relying_Party'  
```  
  
下列預先驗證只有已加入工作場所的裝置：  
  
```  
Add-WebApplicationProxyApplication  
     -BackendServerUrl 'https://mail.contoso.com'   
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'   
     -EnableHTTPRedirect:$true   
     -ExternalUrl 'https://mail.contoso.com'   
     -Name 'Exchange'   
     -ExternalPreAuthentication ADFSforRichClients  
     -ADFSRelyingPartyName 'EAS_Relying_Party'  
```  
  
## <a name="BKMK_1.4"></a>發行使用 OAuth2，例如 Microsoft Store 應用程式的應用程式  
若要發行的 Microsoft Store 應用程式的應用程式，您必須新增至 Federation Service 的應用程式的信賴憑證者信任。  
  
若要允許 Web 應用程式 Proxy 來執行單一登入 (SSO)，以及委派執行認證使用 Kerberos 限制委派，Web Application Proxy 伺服器必須加入網域。 請參閱[規劃 Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD)。  
  
> [!NOTE]  
> Web 應用程式 Proxy 發佈只有支援使用 OAuth 2.0 通訊協定的 Microsoft Store 應用程式。  
  
在 AD FS 管理主控台中，您必須確定 OAuth 端點已啟用 proxy。 若要查看 OAuth 端點是否已啟用 Proxy，請開啟 AD FS 管理主控台，展開 [服務]  ，按一下 [端點]  ，在 [端點]  清單中，找到 OAuth 端點，確定 [Proxy 已啟用]  欄位為 [是]  。  
  
使用 Microsoft Store 應用程式的用戶端的驗證流程如下所述：  
  
> [!NOTE]  
> Web 應用程式 Proxy 重新導向至 AD FS 伺服器，以進行驗證。 因為 Microsoft Store 應用程式不支援重新導向，如果您使用 Microsoft Store 應用程式，就必須設定 AD FS 伺服器使用 Set-webapplicationproxyconfiguration cmdlet 和 OAuthAuthenticationURL 參數的 URL。  
>   
> Microsoft Store 應用程式可以只使用 Windows PowerShell 來發佈。  
  
1.  用戶端嘗試存取已發佈的 web 應用程式使用的 Microsoft Store 應用程式。  
  
2.  應用程式會將 HTTPS 要求傳送至 Web 應用程式 Proxy 所發佈的 URL。  
  
3.  Web 應用程式 Proxy 將 HTTP 401 回應傳回給包含驗證的 AD FS 伺服器的 URL 的應用程式。 此程序稱為 [探索]。  
  
    > [!NOTE]  
    > 如果應用程式知道驗證 AD FS 伺服器的 URL，而且已經有包含 OAuth 權杖和 edge 權杖的組合權杖，步驟 2 和 3 會略過此驗證流程中。  
  
4.  應用程式會將 HTTPS 要求傳送到 AD FS 伺服器。  
  
5.  應用程式會使用 web 驗證代理人來產生的對話方塊，在其中的使用者，請輸入 AD FS 伺服器的認證來驗證。 如需有關 Web 驗證代理人的詳細資訊，請參閱 [Web 驗證代理人](https://msdn.microsoft.com/library/windows/apps/hh750287.aspx)。  
  
6.  驗證成功後，在 AD FS 伺服器會建立包含 OAuth 權杖和 edge 權杖，並將權杖傳送至應用程式的組合權杖。  
  
7.  應用程式會傳送包含組合權杖，Web 應用程式 Proxy 所發佈的 url 的 HTTPS 要求。  
  
8.  Web 應用程式 Proxy 會將組合權杖分割為兩個部分，並驗證 edge 權杖。  
  
9. 如果 edge 權杖是有效的 Web 應用程式 Proxy 將要求轉寄到後端伺服器，只包含 OAuth 權杖。 使用者獲得了已發行 Web 應用程式的存取權。  
  
這個程序描述如何發行 OAuth2 的應用程式。 這種類型的應用程式可以只使用 Windows PowerShell 來發佈。 在開始之前，請確定您已完成下列各項：  
  
-   在 AD FS 管理主控台中建立應用程式的信賴憑證者信任。  
  
-   確保 OAuth 端點已在 AD FS 管理主控台中啟用 proxy，並記下 URL 路徑。  
  
-   驗證 Web 應用程式 Proxy 伺服器上的憑證是適用於您想要發佈應用程式。  
  
#### <a name="to-publish-an-oauth2-app"></a>若要發行 OAuth2 應用程式  
  
1.  在 Web 應用程式 Proxy 伺服器上，在遠端存取管理主控台中，在**瀏覽**窗格中，按一下**Web 應用程式 Proxy**，然後在**工作** 窗格中，按一下**發佈**。  
  
2.  在 [發行新應用程式精靈]  的 [歡迎]  頁面上，按 [下一步]  。  
  
3.  在 [**預先驗證**頁面上，按一下**Active Directory Federation Services (AD FS)** ，然後按一下**下一步]** 。  
  
4.  在 **支援的用戶端**頁面上，選取**OAuth2** ，然後按一下 **下一步**。  
  
5.  在 [信賴憑證者]  頁面上，從信賴憑證者的清單中選取想要發行之應用程式的信賴憑證者，然後按 [下一步]  。  
  
6.  在 [發行設定]  頁面上，執行下列動作，然後按 [下一步]  ：  
  
    -   在 [名稱]  方塊中，輸入易記的應用程式名稱。  
  
        這個名稱僅用於 [遠端存取管理] 主控台中已發行應用程式的清單中。  
  
    -   在 [外部 URL]  方塊中，輸入這個應用程式的外部 URL，例如 https://server1.contoso.com/app1/。  
  
    -   在 [外部憑證]  清單中，選取主體涵蓋外部 URL 的憑證。  
  
        若要確定您的使用者可以存取您的應用程式，即使他們沒有輸入 HTTPS URL 中，選取**啟用的 HTTP 至 HTTPS 重新導向** 方塊中。  
  
    -   在 [後端伺服器 URL]  方塊中，輸入後端伺服器的 URL。 請注意，當您輸入外部 URL 應該只在後端伺服器 URL 不同; 如果變更會自動輸入此值比方說， https://sp/app1/。  
  
        > [!NOTE]  
        > Web 應用程式 Proxy 可以轉譯 Url 中的主機名稱，但是不能轉譯路徑名稱。 因此，您可以輸入不同的主機名稱，但是必須輸入相同的路徑名稱。 例如，您可以輸入外部 URL https://apps.contoso.com/app1/以及後端伺服器 URL 的 https://app-server/app1/。 不過，您無法在此輸入的外部 URL https://apps.contoso.com/app1/以及後端伺服器 URL 的 https://apps.contoso.com/internal-app1/。  
  
7.  在 [確認]  頁面上，檢查設定，然後按一下 [發行]  。 您可以複製 PowerShell 命令來設定其他的已發行應用程式。  
  
8.  在 [結果]  頁面上，確認已成功發行應用程式，然後按一下 [關閉]  。  
  
在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
若要設定同盟伺服器的 OAuth 驗證 URL，位址 fs.contoso.com 和 URL 路徑的 adfs/oauth2//oauth2 /:  
  
```  
Set-WebApplicationProxyConfiguration -OAuthAuthenticationURL 'https://fs.contoso.com/adfs/oauth2/'  
```  
  
發行應用程式：  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerURL 'https://storeapp.contoso.com/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://storeapp.contoso.com/'  
    -Name 'Microsoft Store app Server'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'Store_app_Relying_Party'  
    -UseOAuthAuthentication  
```  
  
## <a name="BKMK_Links"></a>另請參閱  
  
-   [對 Web 應用程式 Proxy 進行疑難](https://technet.microsoft.com/library/dn770156.aspx)排解  
  
-   [透過 Web 應用程式 Proxy 發佈應用程式](https://technet.microsoft.com/library/dn383659.aspx)  
  
-   [規劃使用 Web 應用程式 Proxy 發佈應用程式](https://technet.microsoft.com/library/dn383650.aspx)  
  
-   [Web 應用程式 Proxy 逐步解說指南](https://technet.microsoft.com/library/dn280944.aspx)  
  
-   [Windows PowerShell 中的 web 應用程式 Proxy Cmdlet](https://technet.microsoft.com/library/dn283404.aspx)  
  
-   [Add-WebApplicationProxyApplication](https://technet.microsoft.com/library/dn283409.aspx)  
  
-   [Set-WebApplicationProxyConfiguration](https://technet.microsoft.com/library/dn283406.aspx)  
  



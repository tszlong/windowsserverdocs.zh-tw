---
ms.assetid: 5f733510-c96e-4d3a-85d2-4407de95926e
title: 使用 AD FS 預先驗證發佈應用程式
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server
ms.technology: web-app-proxy
ms.openlocfilehash: bd5c4c97e01942e7c5ab8ed1aba3fcf92030ac59
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404262"
---
# <a name="publishing-applications-using-ad-fs-preauthentication"></a>使用 AD FS 預先驗證發佈應用程式

>適用於︰Windows Server 2016

**此內容與內部部署版本的 Web 應用程式 Proxy 相關。若要在雲端上啟用內部部署應用程式的安全存取，請參閱[Azure AD 應用程式 Proxy 內容](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/)。**  
  
本主題描述如何使用 Active Directory 同盟服務（AD FS）預先驗證，透過 Web 應用程式 Proxy 發佈應用程式。  
  
對於您可以使用 AD FS 預先驗證發行的所有應用程式類型，您必須將 AD FS 信賴憑證者信任新增至同盟服務。  
  
一般 AD FS 預先驗證流程如下所示：  
  
> [!NOTE]  
> 此驗證流程不適用於使用 Microsoft Store 應用程式的用戶端。  
  
1.  用戶端裝置會嘗試存取特定資源 URL 上已發佈的 web 應用程式;例如 https://app1.contoso.com/。  
  
    資源 URL 是 Web 應用程式 Proxy 用來接聽傳入 HTTPS 要求的公用位址。  
  
    如果已啟用 HTTP 至 HTTPS 重新導向，Web 應用程式 Proxy 也會接聽傳入的 HTTP 要求。  
  
2.  Web 應用程式 Proxy 會將 HTTPS 要求重新導向至包含 URL 編碼參數的 AD FS 伺服器，包括資源 URL 和 appRealm （信賴憑證者識別碼）。  
  
    使用者會使用 AD FS 伺服器所需的驗證方法進行驗證;例如，使用者名稱和密碼、具有一次性密碼的雙因素驗證等等。  
  
3.  驗證使用者之後，AD FS 伺服器會發出安全性權杖，即「edge 權杖」，其中包含下列資訊，並將 HTTPS 要求重新導向回 Web 應用程式 Proxy 伺服器：  
  
    -   使用者嘗試存取的資源識別碼。  
  
    -   使用者的身分識別，做為使用者主體名稱（UPN）。  
  
    -   授與核准存取的到期日期；也就是說，只會授與使用者一段限定時間的存取權，過了這個時間，就會要求使用者重新驗證。  
  
    -   edge 權杖資訊的簽章  
  
4.  Web 應用程式 Proxy 會使用 edge 權杖從 AD FS 伺服器接收重新導向的 HTTPS 要求，並驗證並使用權杖，如下所示：  
  
    -   驗證 edge 權杖簽章來自于 Web 應用程式 Proxy 設定中所設定的 federation service。  
  
    -   驗證發行的是正確的應用程式權杖。  
  
    -   驗證權杖尚未過期。  
  
    -   視需要使用使用者身分識別，例如，如果後端伺服器設定要使用整合式 Windows 驗證，就要取得 Kerberos 票證。  
  
5.  如果 edge 權杖有效，Web 應用程式 Proxy 會使用 HTTP 或 HTTPS 將 HTTPS 要求轉寄到發佈的 Web 應用程式。  
  
6.  用戶端現在可以存取發行的 Web 應用程式；不過，已發行的應用程式可能會設定要求使用者執行其他驗證。 例如，如果發行的 Web 應用程式是 SharePoint 網站，而且不需要其他驗證，則使用者會在瀏覽器中看到 SharePoint 網站。  
  
7.  Web 應用程式 Proxy 會將 cookie 儲存在用戶端裝置上。 Web 應用程式 Proxy 會使用 cookie 來識別此會話已預先驗證，而且不需要進一步的預先驗證。  
  
> [!IMPORTANT]  
> 設定外部 URL 和後端伺服器 URL 時，確定您包含完整網域名稱 (FQDN)，而不是 IP 位址。  
  
> [!NOTE]  
> 本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_1.1"></a>發行適用于網頁瀏覽器用戶端的宣告式應用程式  
若要發行使用宣告進行驗證的應用程式，您必須將應用程式的信賴憑證者信任新增至 Federation Service。  
  
發行宣告式應用程式以及從瀏覽器存取應用程式的時候，一般驗證流程如下：  
  
1.  用戶端會嘗試使用網頁瀏覽器來存取宣告式應用程式;例如， https://appserver.contoso.com/claimapp/。  
  
2.  網頁瀏覽器會將 HTTPS 要求傳送至 Web 應用程式 Proxy 伺服器，以將要求重新導向至 AD FS 伺服器。  
  
3.  AD FS 伺服器會驗證使用者和裝置，並將要求重新導向回 Web 應用程式 Proxy。 要求現在包含 edge 權杖。 AD FS 伺服器會將單一登入（SSO） cookie 新增至要求，因為使用者已經對 AD FS 伺服器執行驗證。  
  
4.  Web 應用程式 Proxy 會驗證權杖、新增自己的 cookie，並將要求轉寄到後端伺服器。  
  
5.  後端伺服器會將要求重新導向至 AD FS 伺服器，以取得應用程式安全性權杖。  
  
6.  AD FS 伺服器會將要求重新導向至後端伺服器。 要求現在包含應用程式權杖和 SSO Cookie。 使用者已獲得應用程式的存取權，不需要輸入使用者名稱或密碼。  
  
這個程序描述如何發行網頁瀏覽器用戶端存取的宣告式應用程式 (如 SharePoint 網站)。 在開始之前，請確定您已完成下列各項：  
  
-   在 AD FS 管理主控台中建立應用程式的信賴憑證者信任。  
  
-   確認 Web 應用程式 Proxy 伺服器上的憑證適用于您想要發佈的應用程式。  
  

  
#### <a name="to-publish-a-claims-based-application"></a>發行宣告式應用程式  
  
1.  在 Web 應用程式 Proxy 伺服器上，在 [遠端存取管理] 主控台的**流覽**窗格中，按一下 [ **Web 應用程式 Proxy**]，然後在 **[工作]** 窗格中按一下 [**發佈**]。  
  
2.  在 [發行新應用程式精靈] 的 [歡迎] 頁面上，按 [下一步]。  
  
3.  在 [預先**驗證**] 頁面上，按一下 [ **Active Directory 同盟服務（AD FS）** ]，然後按 **[下一步]** 。  
  
4.  在 [支援的用戶端] 頁面上，選取 [Web 和 MSOFBA]，然後按 [下一步]。  
  
5.  在 [信賴憑證者] 頁面上，從信賴憑證者的清單中選取想要發行之應用程式的信賴憑證者，然後按 [下一步]。  
  
6.  在 [發行設定] 頁面上，執行下列動作，然後按 [下一步]：  
  
    -   在 [名稱] 方塊中，輸入易記的應用程式名稱。  
  
        這個名稱僅用於 [遠端存取管理] 主控台中已發行應用程式的清單中。  
  
    -   在 [外部 URL] 方塊中，輸入這個應用程式的外部 URL，例如 https://sp.contoso.com/app1/。  
  
    -   在 [外部憑證] 清單中，選取主體涵蓋外部 URL 的憑證。  
  
    -   在 [後端伺服器 URL] 方塊中，輸入後端伺服器的 URL。 請注意，當您輸入外部 URL 時，系統會自動輸入此值，而且只有在後端伺服器 URL 不同時，您才應該變更它。例如， https://sp/app1/。  
  
        > [!NOTE]  
        > Web 應用程式 Proxy 可以轉譯 Url 中的主機名稱，但無法轉譯路徑名稱。 因此，您可以輸入不同的主機名稱，但是必須輸入相同的路徑名稱。 例如，您可以輸入 https://apps.contoso.com/app1/ 的外部 URL 和 https://app-server/app1/的後端伺服器 URL。 不過，您不能輸入 https://apps.contoso.com/app1/ 的外部 URL 和 https://apps.contoso.com/internal-app1/的後端伺服器 URL。  
  
7.  在 [確認] 頁面上，檢查設定，然後按一下 [發行]。 您可以複製 PowerShell 命令來設定其他的已發行應用程式。  
  
8.  在 [結果] 頁面上，確認已成功發行應用程式，然後按一下 [關閉]。  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif) ***<em>Windows PowerShell 對等命令</em>***  
  
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
  
## <a name="BKMK_1.2"></a>發行適用于網頁瀏覽器用戶端的整合式 Windows 驗證型應用程式  
Web 應用程式 Proxy 可以用來發行使用整合式 Windows 驗證的應用程式;也就是說，Web 應用程式 Proxy 會視需要執行預先驗證，然後再對使用整合式 Windows 驗證的已發行應用程式執行 SSO。 若要發行使用整合式 Windows 驗證的應用程式，您必須將應用程式的非宣告感知信賴憑證者信任新增至 Federation Service。  
  
若要允許 Web 應用程式 Proxy 執行單一登入（SSO）並使用 Kerberos 限制委派執行認證委派，Web 應用程式 Proxy 伺服器必須加入網域。 請參閱[計畫 Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD)。  
  
若要允許使用者存取使用整合式 Windows 驗證的應用程式，Web 應用程式 Proxy 伺服器必須能夠為使用者提供已發行應用程式的委派。 您可以在網域控制站上為任何應用程式執行這項操作。 如果是在 Windows Server 2012 R2 或 Windows Server 2012 上執行，您也可以在後端伺服器上進行此動作。 如需詳細資訊，請參閱 [Kerberos 驗證的新功能](https://technet.microsoft.com/library/hh831747.aspx)。  
  
如需如何使用整合式 Windows 驗證來設定 Web 應用程式 Proxy 以發行應用程式的逐步解說，請參閱[設定網站以使用整合式 windows 驗證](https://technet.microsoft.com/library/dn280943.aspx#BKMK_3)。  
  
對後端伺服器使用整合式 Windows 驗證時，Web 應用程式 Proxy 與已發行應用程式之間的驗證不是宣告式，而是使用 Kerberos 限制委派來驗證應用程式的使用者。 一般流程如下所述：  
  
1.  用戶端嘗試使用網頁瀏覽器來存取非宣告式應用程式;例如， https://appserver.contoso.com/nonclaimapp/。  
  
2.  網頁瀏覽器會將 HTTPS 要求傳送至 Web 應用程式 Proxy 伺服器，以將要求重新導向至 AD FS 伺服器。  
  
3.  AD FS 伺服器會驗證使用者，並將要求重新導向回 Web 應用程式 Proxy。 要求現在包含 edge 權杖。  
  
4.  Web 應用程式 Proxy 會驗證權杖。  
  
5.  如果權杖有效，Web 應用程式 Proxy 會代表使用者從網域控制站取得 Kerberos 票證。  
  
6.  Web 應用程式 Proxy 會將 Kerberos 票證新增到要求中做為簡單且受保護的 GSS-API 協調機制（SPNEGO）權杖的一部分，然後將要求轉送至後端伺服器。 要求包含 Kerberos 票證，因此，使用者取得應用程式的存取權，且不需要進一步驗證。  
  
這個程序描述如何發行網頁瀏覽器用戶端存取的整合式 Windows 驗證應用程式 (如 Outlook Web 應用程式)。 在開始之前，請確定您已完成下列各項：  
  
-   在 AD FS 管理主控台中，建立應用程式的非宣告感知信賴憑證者信任。  
  
-   設定後端伺服器以便在網域控制站上支援 Kerberos 限制委派，或使用 Set-ADUser Cmdlet 搭配 -PrincipalsAllowedToDelegateToAccount 參數。 請注意，如果後端伺服器是在 Windows Server 2012 R2 或 Windows Server 2012 上執行，您也可以在後端伺服器上執行這個 PowerShell 命令。  
  
-   確定 Web 應用程式 Proxy 伺服器已設定委派給後端伺服器的服務主體名稱。  
  
-   確認 Web 應用程式 Proxy 伺服器上的憑證適用于您想要發佈的應用程式。  
  
 
  
#### <a name="to-publish-a-non-claims-based-application"></a>發行非宣告式應用程式  
  
1.  在 Web 應用程式 Proxy 伺服器上，在 [遠端存取管理] 主控台的**流覽**窗格中，按一下 [ **Web 應用程式 Proxy**]，然後在 **[工作]** 窗格中按一下 [**發佈**]。  
  
2.  在 [發行新應用程式精靈] 的 [歡迎] 頁面上，按 [下一步]。  
  
3.  在 [預先**驗證**] 頁面上，按一下 [ **Active Directory 同盟服務（AD FS）** ]，然後按 **[下一步]** 。  
  
4.  在 [支援的用戶端] 頁面上，選取 [Web 和 MSOFBA]，然後按 [下一步]。  
  
5.  在 [信賴憑證者] 頁面上，從信賴憑證者的清單中選取想要發行之應用程式的信賴憑證者，然後按 [下一步]。  
  
6.  在 [發行設定] 頁面上，執行下列動作，然後按 [下一步]：  
  
    -   在 [名稱] 方塊中，輸入易記的應用程式名稱。  
  
        這個名稱僅用於 [遠端存取管理] 主控台中已發行應用程式的清單中。  
  
    -   在 [外部 URL] 方塊中，輸入這個應用程式的外部 URL，例如 https://owa.contoso.com/。  
  
    -   在 [外部憑證] 清單中，選取主體涵蓋外部 URL 的憑證。  
  
    -   在 [後端伺服器 URL] 方塊中，輸入後端伺服器的 URL。 請注意，當您輸入外部 URL 時，系統會自動輸入此值，而且只有在後端伺服器 URL 不同時，您才應該變更它。例如， https://owa/。  
  
        > [!NOTE]  
        > Web 應用程式 Proxy 可以轉譯 Url 中的主機名稱，但無法轉譯路徑名稱。 因此，您可以輸入不同的主機名稱，但是必須輸入相同的路徑名稱。 例如，您可以輸入 https://apps.contoso.com/app1/ 的外部 URL 和 https://app-server/app1/的後端伺服器 URL。 不過，您不能輸入 https://apps.contoso.com/app1/ 的外部 URL 和 https://apps.contoso.com/internal-app1/的後端伺服器 URL。  
  
    -   在 [後端伺服器 SPN] 方塊中，輸入後端伺服器的服務主體名稱，例如，HTTP/owa.contoso.com。  
  
7.  在 [確認] 頁面上，檢查設定，然後按一下 [發行]。 您可以複製 PowerShell 命令來設定其他的已發行應用程式。  
  
8.  在 [結果] 頁面上，確認已成功發行應用程式，然後按一下 [關閉]。  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif) ***<em>Windows PowerShell 對等命令</em>***  
  
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
  
## <a name="BKMK_1.3"></a>發行使用 OFBA 的應用程式  
Web 應用程式 Proxy 支援從存取後端伺服器上的檔和資料的 Microsoft Office 用戶端（例如 Microsoft Word）存取。 這些應用程式與標準瀏覽器之間唯一的差別在於，重新導向至 STS 的作業不是透過一般 HTTP 重新導向來完成，而是使用中指定的特殊 OFBA 標頭： [https://msdn.microsoft.com/library/dd773463(v=office.12).aspx](https://msdn.microsoft.com/library/dd773463(v=office.12).aspx)。 後端應用程式可以是宣告或 IWA。   
若要為使用 OFBA 的用戶端發行應用程式，您必須將應用程式的信賴憑證者信任新增至同盟服務。 取決於應用程式，您可以使用宣告式驗證或整合式 Windows 驗證。 所以，您必須根據應用程式來新增相關的信賴憑證者信任。  
  
若要允許 Web 應用程式 Proxy 執行單一登入（SSO）並使用 Kerberos 限制委派執行認證委派，Web 應用程式 Proxy 伺服器必須加入網域。 請參閱[計畫 Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD)。  
  
如果應用程式使用宣告型驗證，則沒有任何額外的規劃步驟。 如果應用程式使用整合式 Windows 驗證，請參閱[發行適用于網頁瀏覽器用戶端的整合式 windows 驗證型應用程式](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2)。  
  
使用以宣告為基礎之驗證的 OFBA 通訊協定用戶端的驗證流程如下所述。 這個案例的驗證可以在 URL 或內文中使用應用程式權杖。  
  
1.  使用者正在使用 Office 程式，他從 [最近的文件] 清單中，開啟 SharePoint 網站上的一個檔案。  
  
2.  Office 程式會顯示一個包含瀏覽控制項的視窗，要求使用者輸入認證。  
  
    > [!NOTE]  
    > 在某些情況下視窗可能不會出現，因為用戶端已經過驗證。  
  
3.  Web 應用程式 Proxy 會將要求重新導向至 AD FS 伺服器，以執行驗證。  
  
4.  AD FS 伺服器會將要求重新導向回 Web 應用程式 Proxy。 要求現在包含 edge 權杖。  
  
5.  AD FS 伺服器會將單一登入（SSO） cookie 新增至要求，因為使用者已經對 AD FS 伺服器執行驗證。  
  
6.  Web 應用程式 Proxy 會驗證權杖，並將要求轉送到後端伺服器。  
  
7.  後端伺服器會將要求重新導向至 AD FS 伺服器，以取得應用程式安全性權杖。  
  
8.  要求已重新導向到後端伺服器。 要求現在包含應用程式權杖和 SSO Cookie。 使用者已獲得 SharePoint 網站的存取權，不需要輸入使用者名稱或密碼就能檢視檔案。  
  
發佈使用 OFBA 之應用程式的步驟，與宣告式應用程式或非宣告式應用程式的步驟相同。 針對以宣告為基礎的應用程式，請參閱發行適用于[網頁瀏覽器用戶端的宣告式應用程式](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.1)，對於非宣告式應用程式，請參閱[發行適用于網頁瀏覽器用戶端的整合式 Windows 驗證型應用程式](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2)。 Web 應用程式 Proxy 會自動偵測用戶端，並視需要驗證使用者。  
  
## <a name="publish-an-application-that-uses-http-basic"></a>發行使用 HTTP Basic 的應用程式  

HTTP Basic 是許多通訊協定所使用的授權通訊協定，可透過您的 Exchange 信箱連接豐富型用戶端（包括 smartphone）。 如需有關 HTTP Basic 的詳細資訊，請參閱[RFC 2617](https://www.ietf.org/rfc/rfc2617.txt)。 Web 應用程式 Proxy 傳統上會使用重新導向來與 AD FS 互動;大部分豐富的用戶端不支援 cookie 或狀態管理。 如此一來，Web 應用程式 Proxy 就可以讓 HTTP 應用程式接收到同盟服務的非宣告信賴憑證者信任。 請參閱[計畫 Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD)。  
  
使用 HTTP Basic 之用戶端的驗證流程如下圖所述：  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/WebApplicationProxy_httpBasicflow.png)  
  
1.  使用者嘗試存取已發佈的 web 應用程式電話用戶端。  
  
2.  應用程式會將 HTTPS 要求傳送至 Web 應用程式 Proxy 所發佈的 URL。  
  
3.  如果要求不包含認證，Web 應用程式 Proxy 會將 HTTP 401 回應傳回給包含驗證 AD FS 伺服器之 URL 的應用程式。  
  
4.  使用者會再次將 HTTPS 要求傳送至應用程式，並將授權設定為 [基本] 和 [使用者名稱] 和 [在 www 驗證要求標頭中使用者的 Base 64 加密密碼]。  
  
5.  由於裝置無法重新導向至 AD FS，因此 Web 應用程式 Proxy 會將驗證要求傳送至具有認證的 AD FS，其中包含使用者名稱和密碼。 會代表裝置取得權杖。  
  
6.  為了盡可能減少傳送至 AD FS 的要求數，Web 應用程式 Proxy 會使用快取的權杖來驗證後續的用戶端要求，只要權杖有效。 Web 應用程式 Proxy 會定期清除快取。 您可以使用效能計數器來查看快取的大小。  
  
7.  如果權杖有效，Web 應用程式 Proxy 就會將要求轉送到後端伺服器，並將已發佈 Web 應用程式的存取權授與使用者。  
  
下列程式說明如何發行 HTTP 基本應用程式。  
  
#### <a name="to-publish-an-http-basic-application"></a>發行 HTTP 基本應用程式  
  
1.  在 Web 應用程式 Proxy 伺服器上，在 [遠端存取管理] 主控台的**流覽**窗格中，按一下 [ **Web 應用程式 Proxy**]，然後在 **[工作]** 窗格中按一下 [**發佈**]。  
  
2.  在 [發行新應用程式精靈] 的 [歡迎] 頁面上，按 [下一步]。  
  
3.  在 [預先**驗證**] 頁面上，按一下 [ **Active Directory 同盟服務（AD FS）** ]，然後按 **[下一步]** 。  
  
4.  在**支援的用戶端**頁面上，選取 [ **HTTP 基本**]，然後按 **[下一步]** 。  
  
    如果您只想要從已加入工作地點的裝置存取 Exchange，請選取 [**僅針對加入工作區的裝置啟用存取**] 方塊。 如需詳細資訊[，請參閱從任何裝置加入工作場所以進行 SSO，以及跨公司應用程式進行無縫第二因素驗證](https://technet.microsoft.com/library/dn280945.aspx)。  
  
5.  在 [信賴憑證者] 頁面上，從信賴憑證者的清單中選取想要發行之應用程式的信賴憑證者，然後按 [下一步]。 請注意，這份清單僅包含宣告中的信賴憑證者。  
  
6.  在 [發行設定] 頁面上，執行下列動作，然後按 [下一步]：  
  
    -   在 [名稱] 方塊中，輸入易記的應用程式名稱。  
  
        這個名稱僅用於 [遠端存取管理] 主控台中已發行應用程式的清單中。  
  
    -   在 [**外部 url** ] 方塊中，輸入此應用程式的外部 url。例如，mail.contoso.com  
  
    -   在 [外部憑證] 清單中，選取主體涵蓋外部 URL 的憑證。  
  
    -   在 [後端伺服器 URL] 方塊中，輸入後端伺服器的 URL。 請注意，當您輸入外部 URL 時，系統會自動輸入此值，而且只有在後端伺服器 URL 不同時，您才應該變更它。例如，mail.contoso.com。  
  
7.  在 [確認] 頁面上，檢查設定，然後按一下 [發行]。 您可以複製 PowerShell 命令來設定其他的已發行應用程式。  
  
8.  在 [結果] 頁面上，確認已成功發行應用程式，然後按一下 [關閉]。  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif) ***<em>Windows PowerShell 對等命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
這個 Windows PowerShell 腳本會啟用所有裝置的預先驗證，而不只是加入工作地點的裝置。  
  
```  
Add-WebApplicationProxyApplication  
     -BackendServerUrl 'https://mail.contoso.com'   
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'   
     -ExternalUrl 'https://mail.contoso.com'   
     -Name 'Exchange'   
     -ExternalPreAuthentication ADFSforRichClients  
     -ADFSRelyingPartyName 'EAS_Relying_Party'  
```  
  
下列如何預先驗證僅限工作場所加入的裝置：  
  
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
  
## <a name="BKMK_1.4"></a>發行使用 OAuth2 的應用程式，例如 Microsoft Store 應用程式  
若要發佈 Microsoft Store 應用程式的應用程式，您必須將應用程式的信賴憑證者信任新增至同盟服務。  
  
若要允許 Web 應用程式 Proxy 執行單一登入（SSO）並使用 Kerberos 限制委派執行認證委派，Web 應用程式 Proxy 伺服器必須加入網域。 請參閱[計畫 Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD)。  
  
> [!NOTE]  
> Web 應用程式 Proxy 僅支援針對使用 OAuth 2.0 通訊協定的 Microsoft Store 應用程式發行。  
  
在 AD FS 管理主控台中，您必須確定 OAuth 端點已啟用 proxy。 若要查看 OAuth 端點是否已啟用 Proxy，請開啟 AD FS 管理主控台，展開 [服務]，按一下 [端點]，在 [端點] 清單中，找到 OAuth 端點，確定 [Proxy 已啟用] 欄位為 [是]。  
  
使用 Microsoft Store 應用程式之用戶端的驗證流程如下所述：  
  
> [!NOTE]  
> Web 應用程式 Proxy 會重新導向至 AD FS 伺服器進行驗證。 由於 Microsoft Store 應用程式不支援重新導向，因此，如果您使用 Microsoft Store apps，則必須使用 Set-webapplicationproxyconfiguration Cmdlet 和 OAuthAuthenticationURL 參數來設定 AD FS 伺服器的 URL。  
>   
> Microsoft Store 應用程式只能使用 Windows PowerShell 來發行。  
  
1.  用戶端會使用 Microsoft Store 應用程式嘗試存取已發佈的 web 應用程式。  
  
2.  應用程式會將 HTTPS 要求傳送至 Web 應用程式 Proxy 所發佈的 URL。  
  
3.  Web 應用程式 Proxy 會將 HTTP 401 回應傳回給包含驗證 AD FS 伺服器之 URL 的應用程式。 此程式稱為「探索」。  
  
    > [!NOTE]  
    > 如果應用程式知道驗證 AD FS 伺服器的 URL，而且已經有包含 OAuth 權杖和 edge 權杖的組合 token，則此驗證流程中會略過步驟2和3。  
  
4.  應用程式會將 HTTPS 要求傳送至 AD FS 伺服器。  
  
5.  應用程式會使用 web 驗證代理人來產生對話方塊，使用者可以在其中輸入認證以向 AD FS 伺服器進行驗證。 如需有關 Web 驗證代理人的詳細資訊，請參閱 [Web 驗證代理人](https://msdn.microsoft.com/library/windows/apps/hh750287.aspx)。  
  
6.  成功驗證之後，AD FS 伺服器會建立包含 OAuth 權杖和 edge 權杖的組合權杖，並將權杖傳送至應用程式。  
  
7.  應用程式會將包含組合權杖的 HTTPS 要求傳送至 Web 應用程式 Proxy 所發佈的 URL。  
  
8.  Web 應用程式 Proxy 會將組合權杖分割為兩個部分，並驗證 edge 權杖。  
  
9. 如果 edge 權杖有效，Web 應用程式 Proxy 會將要求轉送至只有 OAuth 權杖的後端伺服器。 使用者獲得了已發行 Web 應用程式的存取權。  
  
這個程序描述如何發行 OAuth2 的應用程式。 這種類型的應用程式只能使用 Windows PowerShell 來發行。 在開始之前，請確定您已完成下列各項：  
  
-   在 AD FS 管理主控台中建立應用程式的信賴憑證者信任。  
  
-   確定 OAuth 端點已在 AD FS 管理主控台中啟用 proxy，並記下 URL 路徑。  
  
-   確認 Web 應用程式 Proxy 伺服器上的憑證適用于您想要發佈的應用程式。  
  
#### <a name="to-publish-an-oauth2-app"></a>若要發佈 OAuth2 應用程式  
  
1.  在 Web 應用程式 Proxy 伺服器上，在 [遠端存取管理] 主控台的**流覽**窗格中，按一下 [ **Web 應用程式 Proxy**]，然後在 **[工作]** 窗格中按一下 [**發佈**]。  
  
2.  在 [發行新應用程式精靈] 的 [歡迎] 頁面上，按 [下一步]。  
  
3.  在 [預先**驗證**] 頁面上，按一下 [ **Active Directory 同盟服務（AD FS）** ]，然後按 **[下一步]** 。  
  
4.  在 [**支援的用戶端**] 頁面上，選取**OAuth2** ，然後按 **[下一步]** 。  
  
5.  在 [信賴憑證者] 頁面上，從信賴憑證者的清單中選取想要發行之應用程式的信賴憑證者，然後按 [下一步]。  
  
6.  在 [發行設定] 頁面上，執行下列動作，然後按 [下一步]：  
  
    -   在 [名稱] 方塊中，輸入易記的應用程式名稱。  
  
        這個名稱僅用於 [遠端存取管理] 主控台中已發行應用程式的清單中。  
  
    -   在 [外部 URL] 方塊中，輸入這個應用程式的外部 URL，例如 https://server1.contoso.com/app1/。  
  
    -   在 [外部憑證] 清單中，選取主體涵蓋外部 URL 的憑證。  
  
        為了確保您的使用者可以存取您的應用程式，即使他們不想在 URL 中輸入 HTTPS，請選取 [**啟用 HTTP 至 HTTPS**重新導向] 方塊。  
  
    -   在 [後端伺服器 URL] 方塊中，輸入後端伺服器的 URL。 請注意，當您輸入外部 URL 時，系統會自動輸入此值，而且只有在後端伺服器 URL 不同時，您才應該變更它。例如， https://sp/app1/。  
  
        > [!NOTE]  
        > Web 應用程式 Proxy 可以轉譯 Url 中的主機名稱，但無法轉譯路徑名稱。 因此，您可以輸入不同的主機名稱，但是必須輸入相同的路徑名稱。 例如，您可以輸入 https://apps.contoso.com/app1/ 的外部 URL 和 https://app-server/app1/的後端伺服器 URL。 不過，您不能輸入 https://apps.contoso.com/app1/ 的外部 URL 和 https://apps.contoso.com/internal-app1/的後端伺服器 URL。  
  
7.  在 [確認] 頁面上，檢查設定，然後按一下 [發行]。 您可以複製 PowerShell 命令來設定其他的已發行應用程式。  
  
8.  在 [結果] 頁面上，確認已成功發行應用程式，然後按一下 [關閉]。  
  
在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
若要為同盟伺服器位址 fs.contoso.com 和 URL 路徑/adfs/oauth2/設定 OAuth 驗證 URL：  
  
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
  
-   [規劃使用 Web 應用程式 Proxy 發行應用程式](https://technet.microsoft.com/library/dn383650.aspx)  
  
-   [Web 應用程式 Proxy 逐步解說指南](https://technet.microsoft.com/library/dn280944.aspx)  
  
-   [Windows PowerShell 中的 Web 應用程式 Proxy Cmdlet](https://technet.microsoft.com/library/dn283404.aspx)  
  
-   [新增-Add-webapplicationproxyapplication](https://technet.microsoft.com/library/dn283409.aspx)  
  
-   [設定-Set-webapplicationproxyconfiguration](https://technet.microsoft.com/library/dn283406.aspx)  
  



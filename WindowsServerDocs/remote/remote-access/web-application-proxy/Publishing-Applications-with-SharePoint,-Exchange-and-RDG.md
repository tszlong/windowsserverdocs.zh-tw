---
ms.assetid: 61ed00fd-51c7-4728-91fa-8501de9d8f28
title: 使用 SharePoint、Exchange 及 RDG 發佈應用程式
author: billmath
manager: mtillman
ms.author: billmath
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows-server
ms.technology: web-app-proxy
ms.openlocfilehash: f859d40ed04cc25285212968e6cd186cffe760ae
ms.sourcegitcommit: 5c93c685dca3cafeea916cedcc0f915c528484ca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2020
ms.locfileid: "81119242"
---
# <a name="publishing-applications-with-sharepoint-exchange-and-rdg"></a>使用 SharePoint、Exchange 及 RDG 發佈應用程式

> 適用於︰Windows Server 2016

**此內容與內部部署版本的 Web 應用程式 Proxy 相關。若要在雲端上啟用內部部署應用程式的安全存取，請參閱[Azure AD 應用程式 Proxy 內容](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/)。**

本主題說明透過 Web 應用程式 Proxy 發佈 SharePoint Server、Exchange Server 或遠端桌面閘道（RDP）所需的工作。

> [!NOTE]
> 這是依原樣提供的資訊。  遠端桌面服務支援並建議使用[Azure App Proxy，為內部部署應用程式提供安全的遠端存取](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)。

## <a name="publish-sharepoint-server"></a><a name="BKMK_6.1"></a>發行 SharePoint Server
當 SharePoint 網站設定為宣告式驗證或整合式 Windows 驗證時，您可以透過 Web 應用程式 Proxy 發佈 SharePoint 網站。 如果您想要使用 Active Directory 同盟服務（AD FS）進行預先驗證，則必須使用其中一個嚮導來設定信賴憑證者。

-   如果 SharePoint 網站使用宣告式驗證，您必須使用 [新增信賴憑證者信任精靈] 來設定應用程式的信賴憑證者信任。

-   如果 SharePoint 網站使用整合式 Windows 驗證，您必須使用 [新增非宣告式信賴憑證者信任精靈] 來設定應用程式的信賴憑證者信任。 您可以使用 IWA 搭配宣告式 Web 應用程式，前提是您設定 KDC。

    若要允許使用者使用整合式 Windows 驗證進行驗證，Web 應用程式 Proxy 伺服器必須加入網域。

    您必須將應用程式設定為支援 Kerberos 限制委派。 您可以在網域控制站上為任何應用程式執行這項操作。 您也可以直接在後端伺服器上設定應用程式（如果它是在 Windows Server 2012 R2 或 Windows Server 2012 上執行）。 如需詳細資訊，請參閱 [Kerberos 驗證的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831747(v=ws.11))。 您也必須確定 Web 應用程式 Proxy 伺服器已設定委派給後端伺服器的服務主體名稱。 如需如何使用整合式 Windows 驗證來設定 Web 應用程式 Proxy 以發行應用程式的逐步解說，請參閱[設定網站以使用整合式 windows 驗證](https://docs.microsoft.com/previous-versions/orphan-topics/ws.11/dn308246(v=ws.11))。

如果您的 SharePoint 網站是使用備用存取對應 (AAM) 或主機名稱為網站集合加以設定，您可以使用不同的外部和後端伺服器 URL 來發行您的應用程式。 不過，如果您未使用 AAM 或主機名稱網站集合設定您的 SharePoint 網站，您必須使用相同的外部和後端伺服器 URL。

## <a name="publish-exchange-server"></a><a name="BKMK_6.2"></a>發佈 Exchange Server
下表描述您可以透過 Web 應用程式 Proxy 發佈的 Exchange 服務，以及這些服務支援的預先驗證：


|    Exchange 服務    |                                                                            預先驗證                                                                            |                                                                                                                                       注意事項                                                                                                                                        |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Outlook Web App     | -AD FS 使用非宣告式驗證<br />-傳遞<br />-針對內部部署 Exchange 2013 Service Pak 1 （SP1）使用宣告式驗證的 AD FS |                                                                  如需詳細資訊，請參閱： [使用 AD FS 宣告式驗證搭配 Outlook Web App 和 EAC](https://go.microsoft.com/fwlink/?LinkId=393723)                                                                  |
| Exchange 控制台 |                                                                               傳遞                                                                               |                                                                                                                                                                                                                                                                                    |
|    Outlook 無所不在    |                                                                               傳遞                                                                               | 您必須發行三個 URL，Outlook 無所不在才能正常運作：<p>-自動探索 URL。<br />-Exchange Server 的外部主機名稱;也就是針對要連接的用戶端所設定的 URL。<br />-Exchange Server 的內部 FQDN。 |
|  Exchange ActiveSync   |                                                     傳遞<br/> 使用 HTTP 基本驗證通訊協定 AD FS                                                      |                                                                                                                                                                                                                                                                                    |

若要使用整合式 Windows 驗證來發行 Outlook Web App，您必須使用 [新增非宣告式信賴憑證者信任精靈] 來設定應用程式的信賴憑證者信任。

若要允許使用者使用 Kerberos 限制委派進行驗證，Web 應用程式 Proxy 伺服器必須加入網域。

您必須將應用程式設定為支援 Kerberos 驗證。 此外，您必須將服務主體名稱（SPN）註冊至執行 web 服務的帳戶。 您可以在網域控制站或後端伺服器上執行此動作。 在負載平衡的 Exchange 環境中，這需要使用替代的服務帳戶，請參閱設定[負載平衡的用戶端存取伺服器的 Kerberos 驗證](https://docs.microsoft.com/exchange/configuring-kerberos-authentication-for-load-balanced-client-access-servers-exchange-2013-help)

您也可以直接在後端伺服器上設定應用程式（如果它是在 Windows Server 2012 R2 或 Windows Server 2012 上執行）。 如需詳細資訊，請參閱 [Kerberos 驗證的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831747(v=ws.11))。 您也必須確定 Web 應用程式 Proxy 伺服器已設定委派給後端伺服器的服務主體名稱。

## <a name="publishing-remote-desktop-gateway-through-web-application-proxy"></a>透過 Web 應用程式 Proxy 發佈遠端桌面閘道
如果您想要限制存取您的遠端存取閘道，並新增預先驗證以進行遠端存取，您可以透過 Web 應用程式 Proxy 來推出。 這是很好的方法，可確保您有豐富的 RDG 預先驗證（包括 MFA）。 發行但不預先驗證也是一個選項，可在您的系統中提供單一進入點。

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-pass-through-authentication"></a>如何使用 Web 應用程式 Proxy 傳遞驗證在 RDG 中發佈應用程式

1. 根據您的 RD Web 存取（/rdweb）和 RD 閘道（rpc）角色位於相同伺服器或不同伺服器上，安裝將會有所不同。

2. 如果 RD Web 存取和 RD 閘道角色裝載在相同的 RDG 伺服器上，您可以直接在 Web 應用程式 Proxy （例如）中發佈根 FQDN， https://rdg.contoso.com/。

   您也可以個別發佈兩個虛擬目錄，例如<https://rdg.contoso.com/rdweb/> 和 https://rdg.contoso.com/rpc/。

3. 如果 RD Web 存取和 RD 閘道裝載在不同的 RDG 伺服器上，您就必須個別發佈兩個虛擬目錄。 您可以使用相同或不同的外部 FQDN，例如 https://rdweb.contoso.com/rdweb/ 和 https://gateway.contoso.com/rpc/。

4. 如果外部和內部 FQDN 不同，您不應該在 RDWeb 發行規則上停用要求標頭轉譯。 這可以藉由在 Web 應用程式 Proxy 伺服器上執行下列 PowerShell 腳本來完成，但它應該預設為啟用。

   ```PowerShell
   Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false
   ```

   > [!NOTE]
   > 如果您需要支援諸如 RemoteApp 和桌面連線或 iOS 遠端桌面連線等豐富型用戶端，則不支援預先驗證，因此您必須使用傳遞驗證來發行 RDG。

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-with-pre-authentication"></a>如何使用 Web 應用程式 Proxy 搭配預先驗證，在 RDG 中發佈應用程式

1.  Web 應用程式 Proxy 預先驗證與 RDG 的運作方式，是傳遞 Internet Explorer 所取得的預先驗證 cookie，傳遞至遠端桌面連線用戶端（mstsc）。 然後，遠端桌面連線用戶端（mstsc）會使用此方法。 然後，遠端桌面連線用戶端會使用此來作為驗證證明。

    下列程式會告知集合伺服器將必要的自訂 RDP 屬性包含在傳送給用戶端的遠端應用程式 RDP 檔案中。 這些會告訴用戶端需要預先驗證，並將預先驗證服務器位址的 cookie 傳遞至遠端桌面連線用戶端（mstsc .exe）。 與停用 Web 應用程式 Proxy 應用程式上的 HttpOnly 功能搭配使用，可讓遠端桌面連線用戶端（mstsc）利用透過瀏覽器取得的 Web 應用程式 Proxy cookie。

    RD Web 存取伺服器的驗證仍會使用 RD Web 存取表單登入。 這會提供最少的使用者驗證提示，因為 RD Web 存取登入表單會建立用戶端認證存放區，然後供遠端桌面連線用戶端（mstsc）用於任何後續的遠端應用程式啟動。

2.  首先，在 AD FS 中建立手動信賴憑證者信任，如同您發佈宣告感知應用程式一樣。 這表示您必須建立虛擬信賴憑證者信任，以強制執行預先驗證，如此一來，您就可以取得預先驗證，而不會對已發行的伺服器進行 Kerberos 限制委派。 一旦使用者通過驗證，就會透過傳遞所有其他專案。

    > [!WARNING]
    > 最好是使用委派，但它無法完全解決 mstsc 的 SSO 需求，而委派至/rpc 目錄時卻有問題，因為用戶端預期會處理 RD 閘道驗證本身。

3.  若要建立手動信賴憑證者信任，請遵循 AD FS 管理主控台中的步驟：

    1.  使用 [**新增信賴**憑證者信任] 嚮導

    2.  選取 **[手動輸入信賴憑證者的相關資料**]。

    3.  接受所有預設設定。

    4.  在 [信賴憑證者信任識別碼] 中，輸入您將用於 RDG 存取的外部 FQDN，例如 https://rdg.contoso.com/。

        這是您在 Web 應用程式 Proxy 中發佈應用程式時將使用的信賴憑證者信任。

4.  在 Web 應用程式 Proxy 中發佈網站的根目錄（例如， https://rdg.contoso.com/）。 將預先驗證設定為 AD FS 並使用您在上面建立的信賴憑證者信任。 這會讓/rdweb 和/rpc 使用相同的 Web 應用程式 Proxy 驗證 cookie。

    您可以將/rdweb 和/rpc 發佈為個別的應用程式，甚至是使用不同的已發行伺服器。 您只需要確保發行時使用的信賴憑證者信任與針對信賴憑證者信任發出的 Web 應用程式 Proxy 權杖相同，因此在使用相同的信賴憑證者信任發行的應用程式中有效。

5.  如果外部和內部 FQDN 不同，您不應該在 RDWeb 發行規則上停用要求標頭轉譯。 這可以藉由在 Web 應用程式 Proxy 伺服器上執行下列 PowerShell 腳本來完成，但預設應加以啟用：

    ```PowerShell
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$true
    ```

6.  在 RDG 發行的應用程式上，停用 Web 應用程式 Proxy 中的 HttpOnly cookie 屬性。 若要允許 RDG ActiveX 控制項存取 Web 應用程式 Proxy 驗證 cookie，您必須停用 Web 應用程式 Proxy cookie 上的 HttpOnly 屬性。

    這需要您安裝[適用于 WINDOWS RT 8.1、Windows 8.1 和 Windows Server 2012 R2 （KB3000850）的2014年11月更新彙總套件](https://support.microsoft.com/kb/3000850)。

    安裝此修補程式之後，請在 Web 應用程式 Proxy 伺服器上執行下列 PowerShell 腳本，並指定相關的應用程式名稱：

    ```PowerShell
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableHttpOnlyCookieProtection:$true
    ```

    停用 HttpOnly 可讓 RDG ActiveX 控制項存取 Web 應用程式 Proxy 驗證 cookie。

7.  在集合伺服器上設定相關的 RDG 集合，讓遠端桌面連線用戶端（mstsc）知道 rdp 檔案中需要預先驗證。

    -   在 Windows Server 2012 和 Windows Server 2012 R2 中，您可以在 RDG 收集伺服器上執行下列 PowerShell Cmdlet 來完成這項作業：

        ```PowerShell
        Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s: <https://externalfqdn/rdweb/>`nrequire pre-authentication:i:1"
        ```

        當您以自己的值取代時，請務必移除 < 和 > 的括弧，例如：

        ```PowerShell
        Set-RDSessionCollectionConfiguration -CollectionName "MyAppCollection" -CustomRdpProperty "pre-authentication server address:s: https://rdg.contoso.com/rdweb/`nrequire pre-authentication:i:1"
        ```

    -   在 Windows Server 2008 R2 中：

        1.  使用具有系統管理員許可權的帳戶登入終端機伺服器。

        2.  前往 **開始** >系統**管理工具**  > **終端機服務** > **TS RemoteApp 管理員。**

        3.  在 TS RemoteApp 管理員的 [**總覽**] 窗格中，按一下 [RDP 設定] 旁的 [**變更**]。

        4.  在 [**自訂 Rdp 設定**] 索引標籤的 [自訂 rdp 設定] 方塊中，輸入下列 RDP 設定：

            `pre-authentication server address: s: https://externalfqdn/rdweb/`

            `require pre-authentication:i:1`

        5.  完成後 **，按一下 [** 套用]。

            這會告知集合伺服器將自訂 RDP 屬性包含在傳送給用戶端的 RDP 檔案中。 這些會告訴用戶端需要預先驗證，並將預先驗證服務器位址的 cookie 傳遞給遠端桌面連線的用戶端（mstsc .exe）。 這會結合在 Web 應用程式 Proxy 應用程式上停用 HttpOnly，讓遠端桌面連線用戶端（mstsc）利用透過瀏覽器取得的 Web 應用程式 Proxy 驗證 cookie。

            如需 RDP 的詳細資訊，請參閱設定[TS 閘道 OTP 案例](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731249(v=ws.10))。

## <a name="see-also"></a><a name="BKMK_Links"></a>另請參閱

- [規劃使用 Web 應用程式 Proxy 發行應用程式](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383650(v=ws.11))

- [對 Web 應用程式 Proxy 進行疑難](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))排解

- [Web 應用程式 Proxy 逐步解說指南](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))

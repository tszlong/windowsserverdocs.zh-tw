---
ms.assetid: 61ed00fd-51c7-4728-91fa-8501de9d8f28
title: 使用 SharePoint、Exchange 及 RDG 發佈應用程式
description: ''
author: billmath
manager: mtillman
ms.author: billmath
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: web-app-proxy
ms.openlocfilehash: fd706f61216ab8760d94faf98d651d17b24efc91
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447141"
---
# <a name="publishing-applications-with-sharepoint-exchange-and-rdg"></a>使用 SharePoint、Exchange 及 RDG 發佈應用程式

>適用於：Windows Server 2016

**此內容是關於 Web 應用程式 Proxy 的內部部署版本的項目。若要在雲端上啟用安全存取內部部署應用程式，請參閱[Azure AD Application Proxy 內容](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/)。**  

本主題說明發佈 SharePoint Server、 Exchange Server 或透過 Web Application Proxy 的遠端桌面閘道 (RDP) 所需的工作。  

>[!NOTE]
>這項資訊依現狀-是。  遠端桌面服務支援，並建議您使用[Azure 應用程式 Proxy，以提供安全遠端存取內部部署應用程式](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)。

## <a name="BKMK_6.1"></a>發佈 SharePoint Server  
SharePoint 網站設定宣告型驗證或整合式 Windows 驗證時，您可以發佈 SharePoint 網站，透過 Web Application Proxy。 如果您想要使用預先驗證的 Active Directory Federation Services (AD FS)，您必須設定信賴憑證者的合作對象使用其中一個精靈。  

-   如果 SharePoint 網站使用宣告式驗證，您必須使用 [新增信賴憑證者信任精靈] 來設定應用程式的信賴憑證者信任。  

-   如果 SharePoint 網站使用整合式 Windows 驗證，您必須使用 [新增非宣告式信賴憑證者信任精靈] 來設定應用程式的信賴憑證者信任。 您可以使用 IWA 搭配宣告式 Web 應用程式，前提是您設定 KDC。  

    若要允許使用者使用整合式 Windows 驗證進行驗證，Web Application Proxy 伺服器必須加入網域。  

    您必須將應用程式設定為支援 Kerberos 限制委派。 您可以在網域控制站上為任何應用程式執行這項操作。 您也可以設定應用程式直接在後端伺服器上執行 Windows Server 2012 R2 或 Windows Server 2012 上。 如需詳細資訊，請參閱 [Kerberos 驗證的新功能](https://technet.microsoft.com/library/hh831747.aspx)。 您也必須確定 Web 應用程式 Proxy 伺服器已設定委派給服務主體名稱的後端伺服器。 如何設定 Web 應用程式 Proxy 發佈應用程式使用整合式 Windows 驗證的逐步解說，請參閱[站台設定為使用整合式 Windows 驗證](assetId:///b0788958-627f-450f-877c-209b1bd0db52)。  

如果您的 SharePoint 網站是使用備用存取對應 (AAM) 或主機名稱為網站集合加以設定，您可以使用不同的外部和後端伺服器 URL 來發行您的應用程式。 不過，如果您未使用 AAM 或主機名稱網站集合設定您的 SharePoint 網站，您必須使用相同的外部和後端伺服器 URL。  

## <a name="BKMK_6.2"></a>發佈 Exchange Server  
下表描述您可以透過 Web 應用程式 Proxy 與這些服務支援的預先驗證發佈的 Exchange 服務：  


|    Exchange 服務    |                                                                            預先驗證                                                                            |                                                                                                                                       附註                                                                                                                                        |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Outlook Web App     | 使用非宣告式驗證的 AD FS<br />-傳遞<br />AD FS 使用宣告式驗證進行內部部署 Exchange 2013 Service Pak 1 (SP1) |                                                                  如需詳細資訊，請參閱：[使用 AD FS 宣告式驗證搭配 Outlook Web App 和 EAC](https://go.microsoft.com/fwlink/?LinkId=393723)                                                                  |
| Exchange 控制台 |                                                                               傳遞                                                                               |                                                                                                                                                                                                                                                                                    |
|    Outlook 無所不在    |                                                                               傳遞                                                                               | 您必須發行三個 URL，Outlook 無所不在才能正常運作：<br /><br />-自動探索 URL。<br />Exchange server;-外部主機名稱也就是已針對用戶端連線到的 URL。<br />的 Exchange Server 內部 FQDN。 |
|  Exchange ActiveSync   |                                                     傳遞<br/> 使用 HTTP 基本授權通訊協定的 AD FS                                                      |                                                                                                                                                                                                                                                                                    |

若要使用整合式 Windows 驗證來發行 Outlook Web App，您必須使用 [新增非宣告式信賴憑證者信任精靈] 來設定應用程式的信賴憑證者信任。  

若要允許使用 Kerberos 進行驗證的使用者限制 Web Application Proxy 伺服器必須加入網域的委派。  

您必須設定才能支援 Kerberos 驗證的應用程式。 此外您需要註冊服務主要名稱 (SPN) 的帳戶下執行 web 服務。 在網域控制站或後端伺服器上，您可以這麼做。 在 負載平衡的 Exchange 環境，這項作業需要使用替代的服務帳戶，請參閱[負載平衡的用戶端存取伺服器的設定 Kerberos 驗證](https://technet.microsoft.com/library/ff808312(v=exchg.150).aspx)  

您也可以設定應用程式直接在後端伺服器上執行 Windows Server 2012 R2 或 Windows Server 2012 上。 如需詳細資訊，請參閱 [Kerberos 驗證的新功能](https://technet.microsoft.com/library/hh831747.aspx)。 您也必須確定 Web 應用程式 Proxy 伺服器已設定委派給服務主體名稱的後端伺服器。  

## <a name="publishing-remote-desktop-gateway-through-web-application-proxy"></a>發佈遠端桌面閘道，透過 Web Application Proxy  
如果您想要限制存取您的遠端存取閘道，並新增遠端存取的預先驗證，您可以透過 Web Application Proxy 將其推出。 這是真的很好的方式，藉此確定您有包括 MFA 的 RDG 豐富的預先驗證。 而不需要預先驗證發行也是一個選項，並提供單一到系統的進入點。  

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-pass-through-authentication"></a>如何在使用 Web 應用程式 Proxy 傳遞驗證的 RDG 發佈應用程式  

1. 安裝將會不同，視您的 RD Web 存取 (/ rdweb) 和 RD 閘道 (rpc) 的角色是在相同伺服器上或在不同伺服器上。  

2. 如果 RD Web 存取和 RD 閘道角色裝載在相同的 RDG 伺服器上，您可以只發行 FQDN 的根 Web 應用程式 Proxy 例如 https://rdg.contoso.com/。  

   您也可以發佈兩個虛擬目錄個別例如<https://rdg.contoso.com/rdweb/>和 https://rdg.contoso.com/rpc/。  

3. 如果 RD Web 存取和 RD 閘道器裝載在不同的 RDG 伺服器上，您必須個別發佈兩個虛擬目錄。 您可以使用相同或不同外部 FQDN，例如 https://rdweb.contoso.com/rdweb/和 https://gateway.contoso.com/rpc/。  

4. 如果不同的外部和內部的 FQDN 不應該停用 RDWeb 發佈規則的要求標頭轉譯。 這可以由 Web 應用程式 Proxy 伺服器上執行下列 PowerShell 指令碼，但它應該預設會啟用。

   ```  
   Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false  
   ```  

   > [!NOTE]  
   > 如果您需要支援豐富的用戶端，例如 RemoteApp 和桌面連線或 iOS 的遠端桌面連線，這些並不支援預先驗證，因此您必須發行使用傳遞驗證的 RDG。  

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-with-pre-authentication"></a>如何在使用預先驗證的 Web 應用程式 Proxy 的 RDG 發佈應用程式  

1.  RDG 與 web 應用程式 Proxy 預先驗證運作方式是傳遞 Internet Explorer 正在傳遞至遠端桌面連線用戶端 (mstsc.exe) 來取得的預先驗證 cookie。 這接著會使用遠端桌面連線用戶端 (mstsc.exe)。 這會接著使用遠端桌面連線用戶端驗證的證明。  

    下列程序會要求收集伺服器傳送至用戶端的遠端應用程式的 RDP 檔案中包含必要的自訂 RDP 屬性。 這些會告訴用戶端也預先驗證是必要的伺服器位址以遠端桌面連線用戶端 (mstsc.exe) 來傳遞預先驗證的 cookie。 搭配停用 HttpOnly 功能上的 Web 應用程式 Proxy 應用程式，這可讓遠端桌面連線用戶端 (mstsc.exe)，若要利用透過瀏覽器的 Web 應用程式 Proxy cookie。  

    RD Web 存取伺服器的驗證仍會使用 RD Web 存取表單登入。 這會提供最少的使用者驗證的數字會提示，因為 RD Web 存取登入表單會建立一個用戶端認證存放區，然後供遠端桌面連線用戶端 (mstsc.exe) 的任何後續的遠端應用程式啟動。  

2.  首先，建立手動信賴憑證者信任 AD FS 中，如果您已發行的宣告感知應用程式。 這表示您不必建立 dummy，信賴憑證者信任，有強制執行預先驗證，讓您取得預先驗證發行的伺服器而不需要 Kerberos 限制委派。 一旦使用者驗證，其他所有項目會傳遞。  

    > [!WARNING]  
    > 它看起來最好使用委派，但無法完全解決 mstsc SSO 的需求，並委派給 /rpc 目錄，因為用戶端預期處理本身的 RD 閘道驗證時有問題。  

3.  若要建立手動信賴憑證者信任，請依照下列 AD FS 管理主控台中的步驟：  

    1.  使用**新增信賴憑證者信任**精靈  

    2.  選取 **手動輸入信賴憑證者的合作對象的相關資料**。  

    3.  接受所有預設設定。  

    4.  針對該信賴憑證者信任識別碼中，輸入將使用 RDG 存取，例如外部 FQDN https://rdg.contoso.com/。  

        這是信賴憑證者信任中 Web 應用程式 Proxy 發佈應用程式時，您將使用。  

4.  發佈站台的根目錄 (例如 https://rdg.contoso.com/) Web 應用程式 Proxy 中。 設定 AD fs 的預先驗證，並使用您先前建立的信賴憑證者的合作對象信任。 這可讓 /rdweb 和 /rpc 使用相同的 Web 應用程式 Proxy 的驗證 cookie。  

    您可將 /rdweb 和 /rpc 發佈成個別的應用程式，即使使用不同的已發行的伺服器。 您只需要確保您發行都使用同一個信賴憑證者信任的 Web 應用程式 Proxy 權杖發出信賴憑證者信任，所以所有使用同一個信賴憑證者信任發佈的應用程式。  

5.  如果不同的外部和內部的 FQDN 不應該停用 RDWeb 發佈規則的要求標頭轉譯。 這可以由 Web 應用程式 Proxy 伺服器上執行下列 PowerShell 指令碼，但預設應該啟用它：

    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false 
    ```  

6.  停用 HttpOnly cookie 屬性 RDG 上的 Web 應用程式 proxy 發佈應用程式。 若要讓 Web 應用程式 Proxy 的驗證 cookie 的 RDG ActiveX 控制項存取，您必須停用 Web 應用程式 Proxy 的 cookie 的 HttpOnly 屬性。  

    這需要安裝下列[Web 應用程式 Proxy Hotfix](https://support.microsoft.com/en-gb/kb/3000850)或[ https://support.microsoft.com/en-gb/kb/3000850 ](https://support.microsoft.com/en-gb/kb/3000850)。  

    安裝 hotfix 之後請指定相關的應用程式名稱的 Web 應用程式 Proxy 伺服器上執行下列 PowerShell 指令碼：  

    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableHttpOnlyCookieProtection:$true  
    ```  

    停用 HttpOnly，可讓 Web 應用程式 Proxy 的驗證 cookie 的 RDG ActiveX 控制項存取。  

7.  相關的 RDG 集合在伺服器上設定集合，讓用戶端 (mstsc.exe) 可讓您知道的 rdp 檔案中需要預先驗證的遠端桌面連線。  

    -   Windows Server 2012 和 Windows Server 2012 R2 中這可藉由 RDG 集合伺服器上執行下列 PowerShell cmdlet:  

        ```
        Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s: <https://externalfqdn/rdweb/>`nrequire pre-authentication:i:1"
        ```

        請確定您移除 < 和 > 當您取代您自己的值，例如括號：

        ```
        Set-RDSessionCollectionConfiguration -CollectionName "MyAppCollection" -CustomRdpProperty "pre-authentication server address:s: https://rdg.contoso.com/rdweb/`nrequire pre-authentication:i:1"
        ```

    -   在 Windows Server 2008 R2 中：  

        1.  登入終端機伺服器，使用具有系統管理員權限的帳戶。  

        2.  移至**開始** >**系統管理工具** > **終端機服務** > **TS RemoteApp 管理員。**  

        3.  在 [**概觀**] 窗格的 TS RemoteApp 管理員] 中，RDP 設定旁邊，按一下 [**變更**。  

        4.  在 **自訂 RDP 設定**索引標籤上，在 自訂 RDP 設定 中輸入下列的 RDP 設定：  

            `pre-authentication server address: s: https://externalfqdn/rdweb/`  

            `require pre-authentication:i:1`  

        5.  當您完成之後時，按一下**套用**。  

            這會要求收集伺服器傳送至用戶端的 RDP 檔案中包含自訂 RDP 屬性。 這些會告訴用戶端也預先驗證是必要的伺服器位址給遠端桌面連線用戶端 (mstsc.exe) 來傳遞預先驗證的 cookie。 搭配 Web 應用程式 Proxy 應用程式，在停用 HttpOnly 會允許遠端桌面連線用戶端 (mstsc.exe) 利用透過瀏覽器取得 Web 應用程式 Proxy 的驗證 cookie。  

            如需 RDP 的詳細資訊，請參閱[設定 TS 閘道 OTP 案例](https://technet.microsoft.com/library/cc731249(v=ws.10).aspx)。  

## <a name="BKMK_Links"></a>另請參閱  

-   [規劃使用 Web 應用程式 Proxy 發佈應用程式](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383650(v=ws.11))  

-   [對 Web 應用程式 Proxy 進行疑難](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))排解  

-   [Web 應用程式 Proxy 逐步解說指南](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))  

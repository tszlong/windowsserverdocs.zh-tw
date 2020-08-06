---
title: 對 Web 應用程式 Proxy 進行疑難排解
ms.author: kgremban
author: eross-msft
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server
ms.assetid: a2fef55d-747b-4e20-8f21-5f8807e7ef87
ms.technology: web-app-proxy
ms.openlocfilehash: 063301e5403cb9f671054594dbccbeae3730ffc6
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/05/2020
ms.locfileid: "87769476"
---
# <a name="troubleshooting-web-application-proxy"></a>對 Web 應用程式 Proxy 進行疑難排解

>適用於：Windows Server 2016

**此內容與內部部署版本的 Web 應用程式 Proxy 相關。若要在雲端上啟用內部部署應用程式的安全存取，請參閱[Azure AD 應用程式 Proxy 內容](/azure/active-directory/manage-apps/application-proxy)。**

本節提供 Web 應用程式 Proxy 的疑難排解程式，包括事件說明和解決方案。 有三個位置會顯示錯誤：

-   在 Web 應用程式 Proxy 系統管理員主控台中

    系統管理員主控台中列出的每個事件識別碼都可以在 Windows 事件檢視器中查看，而對應的描述和解決方案則位於下面。

    開啟事件檢視器，並在**應用程式和服務記錄**檔  >  **Microsoft**  >  **Windows**  >  **Web 應用程式 proxy**  >  **管理員**底下尋找與 Web 應用程式 proxy 相關的事件

    ![如事件檢視器所示的系統管理員資訊](media/Troubleshooting-Web-Application-Proxy/WebApplicationProxyTroubleshooting.png)

    如有需要，您可以開啟分析和偵測記錄，然後開啟 Web 應用程式 Proxy 會話記錄檔（位於 Windows 事件檢視器的 \ Microsoft \ Windows \ Web 應用程式 Proxy \ 管理員底下），以取得詳細的記錄。

-   在 PowerShell 錯誤中

    在設定期間遇到的問題事件會顯示在 PowerShell 中。

    所有錯誤都會使用標準 PowerShell 錯誤提示向 PowerShell 使用者呈現。 所有 PowerShell 命令都會記錄為事件。 PowerShell 中發生的所有事件都是以識別碼為12016的 Windows 事件檢視器列出，並在下面的 PowerShell 區段中定義。

-   在最佳做法分析程式中

    [Web 應用程式 Proxy 的最佳作法分析程式](assetId:///995f5cea-e334-4ce6-a14c-6a2d03c0c79a)中會描述這些事件

## <a name="powershell-messages"></a>PowerShell 訊息

|事件或徵兆|可能的原因|解決方法|
|--------------------|------------------|--------------|
| ( "ADFS ProxyTrust-" ) 的信任憑證無效 <WAP machine name>|可能是由下列任何原因所造成：<p>-應用程式 Proxy 機器已關閉太長時間。<br />-Web 應用程式 Proxy 與 AD FS 之間的連線中斷<br />-憑證基礎結構問題<br />-AD FS 機上的變更，或 Web 應用程式 Proxy 與 AD FS 之間的更新進程，並未每8小時執行一次，因此他們需要更新信任<br />-Web 應用程式 Proxy 電腦和 AD FS 的時鐘不會同步處理。|請確定時鐘已同步處理。 執行 Install-webapplicationproxy Cmdlet。|
|在 AD FS 中找不到設定資料|這可能是因為 Web 應用程式 Proxy 尚未完整安裝，或因為 AD FS 資料庫或資料庫損毀而變更。|執行 Install-Install-webapplicationproxy Cmdlet|
|Web 應用程式 Proxy 嘗試從 AD FS 讀取設定時發生錯誤。|這可能表示 AD FS 無法連線，或 AD FS 在嘗試從 AD FS 資料庫讀取設定時發生內部問題。|確認 AD FS 可連線並正常運作。|
|儲存在 AD FS 中的設定資料已損毀，或 Web 應用程式 Proxy 無法加以剖析。<p>或者<p>Web 應用程式 Proxywas 無法從 AD FS 取得信賴憑證者的清單。|如果 AD FS 中的設定資料已修改，就可能發生這種情況。|重新開機 Web 應用程式 Proxyservice。 如果問題持續發生，請執行 Install-webapplicationproxy Cmdlet。|

## <a name="administrator-console-events"></a>系統管理員主控台事件
下列系統管理員主控台事件通常會指示驗證錯誤、不正確 tokes 或過期的 cookie。

|事件或徵兆|可能的原因|解決方法|
|--------------------|------------------|--------------|
|11005<p>Web 應用程式 Proxy 無法使用設定中的密碼來建立 cookie 加密金鑰。|全域設定 "AccessCookiesEncryptionKey" 參數已由 PowerShell 命令變更： Set-webapplicationproxyconfiguration-RegenerateAccessCookiesEncryptionKey|不需要執行任何動作。 已移除問題的 cookie，並將使用者重新導向至 STS 進行驗證。|
|12000<p>Web 應用程式 Proxy 無法檢查至少60分鐘的設定變更|Web 應用程式 Proxy 無法使用 Set-webapplicationproxyconfiguration/應用程式命令來存取 Web 應用程式 Proxy 設定。 這通常是因為沒有與 AD FS 的連線，或需要更新與 AD FS 的信任所造成。|檢查與 AD FS 的連線能力。 若要這麼做，您可以使用連結 HTTPs://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xml確認 AD FS 和 Web 應用程式 Proxy 之間已建立信任。 如果這些解決方案無法運作，請執行 Install-webapplicationproxy Cmdlet。|
|12003<p>Web 應用程式 Proxy 無法剖析存取 cookie。|這可能表示 Web 應用程式 Proxy 和 AD FS 未連線，或未收到相同的設定。|檢查與 AD FS 的連線能力。 若要這麼做，您可以使用連結 HTTPs://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xml確認 AD FS 和 Web 應用程式 Proxy 之間已建立信任。 如果這些解決方案無法運作，請執行 Install-webapplicationproxy Cmdlet。|
|12004<p>Web 應用程式 Proxy 收到具有無效存取 cookie 的要求。|此事件可能表示 Web 應用程式 Proxy 和 AD FS 未連線，或未收到相同的設定。<p>如果您執行了 Set-Set-webapplicationproxyconfiguration-RegenerateAccessCookiesEncryptionKey PowerShell 命令所 chaged 的 "AccessCookiesEncryptionKey" 參數，則此事件是正常的，不需要任何解決步驟。|檢查與 AD FS 的連線能力。 若要這麼做，您可以使用連結 HTTPs://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xml確認 AD FS 和 Web 應用程式 Proxy 之間已建立信任。 如果這些解決方案無法運作，請執行 Install-webapplicationproxy Cmdlet。|
|12008<p>Web 應用程式 Proxy 超過後端伺服器允許的 Kerberos 驗證嘗試次數上限。|此事件可能表示 Web 應用程式 Proxy 與後端應用程式伺服器之間的設定不正確，或兩部電腦上的時間與日期設定發生問題。|後端伺服器已拒絕 Web 應用程式 Proxy 所建立的 Kerberos 票證。 確認 Web 應用程式 Proxy 和後端應用程式伺服器的設定是否正確。<p>請確定 Web 應用程式 Proxy 和後端應用程式伺服器上的時間和日期設定已同步處理。|
|12011<p>Web 應用程式 Proxy 收到具有無效存取 cookie 簽章的要求。|此事件可能表示 Web 應用程式 Proxy 和 AD FS 未連線，或未收到相同的設定。 如果您執行了 Set-Set-webapplicationproxyconfiguration-RegenerateAccessCookiesEncryptionKey PowerShell 命令所 chaged 的 "AccessCookiesEncryptionKey" 參數，則此事件是正常的，不需要任何解決步驟。|檢查與 AD FS 的連線能力。 若要這麼做，您可以使用連結 HTTPs://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xml確認 AD FS 和 Web 應用程式 Proxy 之間已建立信任。 如果這些解決方案無法運作，請執行 Install-webapplicationproxy Cmdlet。|
|12027<p>Proxy 在處理要求時發現意外的錯誤。 提供的名稱不是正確格式的帳戶名稱。|此事件可能表示 Web 應用程式 Proxy 與域控制器伺服器之間的設定不正確，或兩部電腦上的時間與日期設定發生問題。|網域控制站已拒絕 Web 應用程式 Proxy 所建立的 Kerberos 票證。 確認 Web 應用程式 Proxy 和後端應用程式伺服器的設定是否正確，尤其是 SPN 設定。 請確定 Web 應用程式 Proxy 已加入與網域控制站相同的網域，以確保網域控制站會與 Web 應用程式 Proxy 建立信任關係。請確定 Web 應用程式 Proxy 和網域控制站上的時間與日期設定已同步處理。|
|13012<p>Web 應用程式 Proxy 收到不正確 edge 權杖簽章||請確定您已使用[KB 2955164](https://go.microsoft.com/fwlink/?LinkId=400701)更新 Web 應用程式 Proxy。|
|13013<p>Web 應用程式 Proxy 收到包含已過期 edge 權杖的要求。|Web 應用程式 Proxy 和 AD FS 沒有已同步處理的時鐘。|同步處理 Web 應用程式 Proxy 與 AD FS 之間的時鐘。|
|13014<p>Web 應用程式 Proxy 收到具有無效邊緣 token 的要求。 Token 無效，因為無法剖析。|這可能表示 AD FS 設定發生問題。|檢查您的 AD FS 設定，並視需要還原預設設定。|
|13015<p>Web 應用程式 Proxy 收到具有過期存取 cookie 的要求。|這可能表示未同步處理的時鐘。|如果您要使用 Web 應用程式 Proxy 電腦的叢集，請確定電腦的時間和日期已同步處理。|
|13016<p>Web 應用程式 Proxy 無法代表使用者取得 Kerberos 票證，因為邊緣權杖或存取 cookie 中沒有 UPN。|STS 組態有問題。|在 STS 中修正 UPN 宣告設定。|
|13019<p>Web 應用程式 Proxy 無法代表使用者取得 Kerberos 票證，因為發生下列一般 API 錯誤|此事件可能表示 Web 應用程式 Proxy 與域控制器伺服器之間的設定不正確，或兩部電腦上的時間與日期設定發生問題。|網域控制站已拒絕 Web 應用程式 Proxy 所建立的 Kerberos 票證。 確認 Web 應用程式 Proxy 和後端應用程式伺服器的設定是否正確，尤其是 SPN 設定。 請確定 Web 應用程式 Proxy 已加入與網域控制站相同的網域，以確保網域控制站會與 Web 應用程式 Proxy 建立信任關係。請確定 Web 應用程式 Proxy 和網域控制站上的時間與日期設定已同步處理。|
|13020<p>Web 應用程式 Proxy 無法代表使用者取得 Kerberos 票證，因為未定義後端伺服器 SPN。|此事件可能表示 Web 應用程式 Proxy 與域控制器伺服器之間的設定不正確，或兩部電腦上的時間與日期設定發生問題。|網域控制站已拒絕 Web 應用程式 Proxy 所建立的 Kerberos 票證。 確認 Web 應用程式 Proxy 和後端應用程式伺服器的設定是否正確，尤其是 SPN 設定。 請確定 Web 應用程式 Proxy 已加入與網域控制站相同的網域，以確保網域控制站會與 Web 應用程式 Proxy 建立信任關係。請確定 Web 應用程式 Proxy 和網域控制站上的時間與日期設定已同步處理。|
|13022<p>Web 應用程式 Proxy 無法驗證使用者，因為後端伺服器會以 HTTP 401 錯誤回應 Kerberos 驗證嘗試。|此事件可能表示 Web 應用程式 Proxy 與後端應用程式伺服器之間的設定不正確，或兩部電腦上的時間與日期設定發生問題。|後端伺服器已拒絕 Web 應用程式 Proxy 所建立的 Kerberos 票證。 確認 Web 應用程式 Proxy 和後端應用程式伺服器的設定是否正確。請確定 Web 應用程式 Proxy 和後端應用程式伺服器上的時間和日期設定已同步處理。|
|13025<p>用戶端未向 Web 應用程式 Proxy 出示 SSL 憑證。|此事件可能表示時間與日期設定中有問題。|請確定憑證基礎結構有效，而且 Web 應用程式 Proxy 和 AD FS 的時間和日期已同步處理。 請確定為 Web 應用程式 Proxy 設定的指紋是正確的。|
|13026<p>用戶端向 Web 應用程式 Proxy 出示了 SSL 憑證，但憑證無效：憑證不符合指紋。|此事件可能表示時間與日期設定中有問題。|請確定憑證基礎結構有效，而且 Web 應用程式 Proxy 和 AD FS 的時間和日期已同步處理。 請確定為 Web 應用程式 Proxy 設定的指紋是正確的。|
|13028<p>Web 應用程式 Proxy 收到包含尚未生效之 edge 權杖的要求。|此事件可能表示時間與日期設定中有問題。|請確定憑證基礎結構有效，而且 Web 應用程式 Proxy 和 AD FS 的時間和日期已同步處理。|
|13030<p>用戶端向 Web 應用程式 Proxy 呈現 SSL 憑證，但信任提供者不信任發行用戶端憑證的憑證授權單位單位。|此事件可能表示時間與日期設定中有問題。|請確定憑證基礎結構有效，而且 Web 應用程式 Proxy 和 AD FS 的時間和日期已同步處理。 請確定為 Web 應用程式 Proxy 設定的指紋是正確的。|
|13031<p>用戶端向 Web 應用程式 Proxy 呈現 SSL 憑證，但憑證鏈在信任提供者不信任的根憑證中終止。|此事件可能表示時間與日期設定中有問題。|請確定憑證基礎結構有效，而且 Web 應用程式 Proxy 和 AD FS 的時間和日期已同步處理。 請確定為 Web 應用程式 Proxy 設定的指紋是正確的。|
|13032<p>用戶端向 Web 應用程式 Proxy 出示了 SSL 憑證，但憑證對要求的使用方式無效。|此事件可能表示時間與日期設定中有問題。|請確定憑證基礎結構有效，而且 Web 應用程式 Proxy 和 AD FS 的時間和日期已同步處理。 請確定為 Web 應用程式 Proxy 設定的指紋是正確的。|
|13033<p>用戶端向 Web 應用程式 Proxy 出示了 SSL 憑證，但是在根據目前的系統時鐘或簽署檔案中的時間戳記進行驗證時，憑證不在其有效期間內。|此事件可能表示時間與日期設定中有問題。|請確定憑證基礎結構有效，而且 Web 應用程式 Proxy 和 AD FS 的時間和日期已同步處理。 請確定為 Web 應用程式 Proxy 設定的指紋是正確的。|
|13034<p>用戶端向 Web 應用程式 Proxy 出示了 SSL 憑證，但憑證無效。|此事件可能表示時間與日期設定中有問題。|請確定憑證基礎結構有效，而且 Web 應用程式 Proxy 和 AD FS 的時間和日期已同步處理。 請確定為 Web 應用程式 Proxy 設定的指紋是正確的。|

下列系統管理員主控台事件通常表示與設定有關的問題，例如布建、未成功的要求、無法連線的後端伺服器，以及緩衝區溢位。

|事件或徵兆|可能的原因|解決方法|
|--------------------|------------------|--------------|
|12019<p>Web Application Proxy 無法為下列 URL 建立接聽程式。|事件的可能原因是另一個服務正在接聽相同的 URL。|系統管理員必須確定沒有人接聽或系結至相同的 Url。 若要檢查此動作，請執行下列命令： netsh HTTP show urlacl。 如果此 URL 是由 Web 應用程式 Proxy 電腦上執行的另一個元件所使用，請將它移除，或使用不同的 URL 透過 Web 應用程式 Proxy 發佈應用程式。|
|12020<p>Web 應用程式 Proxy 無法為下列 URL 建立保留區。|事件的可能原因是另一個服務在相同的 URL 上有一個保留。|系統管理員必須確定沒有任何人系結至相同的 Url。 若要檢查此動作，請執行下列命令： netsh HTTP show urlacl。 如果此 URL 是由 Web 應用程式 Proxy 電腦上執行的另一個元件所使用，請將它移除，或使用不同的 URL 透過 Web 應用程式 Proxy 發佈應用程式。|
|12021<p>Web 應用程式 Proxy 無法系結 SSL 伺服器憑證。 已套用所有其他設定。|無法建立和設定 SSL 憑證資料的設定記錄。|請確定為 Web 應用程式 Proxyapplications 設定的憑證指紋已安裝在本機電腦存放區中具有私密金鑰的所有 Web 應用程式 Proxy 電腦上。|
|13001<p>後端伺服器向 Web 應用程式 Proxy 呈現的 SSL 伺服器憑證無效;憑證不受信任。|在伺服器所傳送的安全通訊端層 (SSL) 憑證中找到一或多個錯誤。 這可能表示後端伺服器提供了不正確 SSL，或 Web 應用程式 Proxy 與後端伺服器之間沒有信任關係。|驗證後端伺服器 SSL 憑證。 請確定已將 Web 應用程式 Proxy 電腦設定為使用正確的根 Ca，以信任後端伺服器憑證。|
|13006|0x80072ee7 錯誤碼時，failurrre 是因無法解析後端伺服器 URL 所造成。 其他錯誤碼如下所述[https://msdn.microsoft.com/library/windows/desktop/aa384110(v=vs.85)](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))|請檢查後端伺服器 URL 是否正確，以及其名稱是否可以從 Web 應用程式 Proxy 電腦正確解析。|
|13007<p>在預期的間隔內，未收到來自後端伺服器的 HTTP 回應。|後端伺服器要求超時或變慢或沒有回應。|檢查後端伺服器設定。 如果速度非常慢，請檢查後端伺服器的連線能力，並考慮變更 InactiveTransactionsTimeoutSec 的 Web 應用程式 Proxy 全域設定參數 Cmdlet。|

## <a name="see-also"></a>另請參閱
[Windows Server 2016 中 Web 應用程式 Proxy 的新功能](web-application-proxy-windows-server.md) 
使用[Web 應用程式 Proxy](assetId:///b607b717-2172-4271-98d1-fa8162e0bb2e)


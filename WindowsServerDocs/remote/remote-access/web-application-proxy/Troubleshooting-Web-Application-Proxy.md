---
description: 深入瞭解：疑難排解 Web 應用程式 Proxy
title: 對 Web 應用程式 Proxy 進行疑難排解
ms.author: kgremban
author: eross-msft
ms.date: 07/13/2016
ms.topic: article
ms.assetid: a2fef55d-747b-4e20-8f21-5f8807e7ef87
ms.openlocfilehash: 6c0ba93afe21c1a1c9c24b3986afa6628690197a
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044886"
---
# <a name="troubleshooting-web-application-proxy"></a>對 Web 應用程式 Proxy 進行疑難排解

>適用於：Windows Server 2016

**此內容與內部部署版本的 Web 應用程式 Proxy 相關。若要透過雲端啟用對內部部署應用程式的安全存取，請參閱 [Azure AD 應用程式 Proxy 內容](/azure/active-directory/manage-apps/application-proxy)。**

本節提供 Web 應用程式 Proxy 的疑難排解程式，包括事件說明與解決方案。 有三個位置會顯示錯誤：

-   在 Web 應用程式 Proxy 系統管理員主控台中

    您可以在 Windows 事件檢視器中查看 [系統管理員主控台] 中列出的每個事件識別碼，並在下方找到對應的描述與解決方案。

    開啟事件檢視器，並在 [**應用程式及服務記錄** 檔  >  **Microsoft**  >  **Windows**  >  **web 應用程式 proxy**  >  **管理員**] 下尋找與 Web 應用程式 Proxy 相關的事件

    ![系統管理員資訊，如事件檢視器](media/Troubleshooting-Web-Application-Proxy/WebApplicationProxyTroubleshooting.png)

    如有需要，開啟分析和偵錯工具記錄檔，並開啟 Web 應用程式 Proxy 會話記錄檔（位於 Windows 事件檢視器的 [\ Microsoft \ Windows \ Web 應用程式 Proxy \ 管理員] 下），即可取得詳細記錄。

-   PowerShell 錯誤

    在設定期間發生之問題的事件會顯示在 PowerShell 中。

    所有錯誤都會使用標準的 PowerShell 錯誤提示，向 PowerShell 使用者呈現。 所有 PowerShell 命令都會記錄為事件。 PowerShell 中發生的所有事件都會列在識別碼為12016的 Windows 事件檢視器中，並在以下的 PowerShell 區段中定義。

-   在最佳做法分析程式中

    這些事件會在[Web 應用程式 Proxy 的最佳做法分析程式](assetId:///995f5cea-e334-4ce6-a14c-6a2d03c0c79a)中說明

## <a name="powershell-messages"></a>PowerShell 訊息

|事件或徵兆|可能的原因|解決方案|
|--------------------|------------------|--------------|
|信任憑證 ( "ADFS ProxyTrust- <WAP machine name> " ) 無效|可能是由下列任何原因所造成：<p>-應用程式 Proxy 電腦的時間太長。<br />-Web 應用程式 Proxy 與 AD FS 之間的中斷連線<br />-憑證基礎結構問題<br />-AD FS 電腦上的變更，或 Web 應用程式 Proxy 與 AD FS 之間的更新程式未在每8小時執行一次，則需要更新信任<br />-Web 應用程式 Proxy 電腦和 AD FS 的時鐘未同步處理。|請確定時鐘已同步處理。 執行 Install-WebApplicationProxy Cmdlet。|
|在 AD FS 中找不到設定資料|這可能是因為 Web 應用程式 Proxy 尚未完整安裝，或由於 AD FS 資料庫或資料庫損毀而造成變更。|執行 Install-WebApplicationProxy Cmdlet|
|當 Web 應用程式 Proxy 嘗試從 AD FS 讀取設定時發生錯誤。|這可能表示無法連接 AD FS，或 AD FS 在嘗試從 AD FS 資料庫讀取設定時發生內部問題。|確認 AD FS 可連線並正常運作。|
|儲存在 AD FS 中的設定資料已損毀，或 Web 應用程式 Proxy 無法加以剖析。<p>或者<p>Web 應用程式 Proxywas 無法從 AD FS 取得信賴憑證者的清單。|如果在 AD FS 中修改了設定資料，就可能會發生這種情況。|重新開機 Web 應用程式 Proxyservice。 如果問題持續發生，請執行 Install-WebApplicationProxy Cmdlet。|

## <a name="administrator-console-events"></a>系統管理員主控台事件
下列系統管理員主控台事件通常表示驗證錯誤、不正確 tokes 或已過期的 cookie。

|事件或徵兆|可能的原因|解決方案|
|--------------------|------------------|--------------|
|11005<p>Web 應用程式 Proxy 無法使用設定中的密碼建立 cookie 加密金鑰。|PowerShell 命令已變更 global configuration "AccessCookiesEncryptionKey" 參數： Set-WebApplicationProxyConfiguration-RegenerateAccessCookiesEncryptionKey|不需要採取任何動作。 已移除有問題的 cookie，並將使用者重新導向至 STS 進行驗證。|
|12000<p>Web 應用程式 Proxy 無法檢查至少60分鐘的設定變更|Web 應用程式 Proxy 無法使用命令 Get-Set-webapplicationproxyconfiguration/Application 來存取 Web 應用程式 Proxy 設定。 這通常是因為缺乏與 AD FS 的連線，或需要更新 AD FS 的信任。|檢查與 AD FS 的連線能力。 您可以使用連結 HTTPs://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xml確定已在 AD FS 與 Web 應用程式 Proxy 之間建立信任。 如果這些解決方案無法運作，請執行 Install-WebApplicationProxy Cmdlet。|
|12003<p>Web 應用程式 Proxy 無法剖析存取 cookie。|這可能表示 Web 應用程式 Proxy 和 AD FS 未連線，或是未收到相同的設定。|檢查與 AD FS 的連線能力。 您可以使用連結 HTTPs://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xml確定已在 AD FS 與 Web 應用程式 Proxy 之間建立信任。 如果這些解決方案無法運作，請執行 Install-WebApplicationProxy Cmdlet。|
|12004<p>Web 應用程式 Proxy 收到具有無效存取 cookie 的要求。|此事件可能表示 Web 應用程式 Proxy 和 AD FS 未連線，或是未收到相同的設定。<p>如果您執行 "AccessCookiesEncryptionKey" 參數是由 Set-WebApplicationProxyConfiguration-RegenerateAccessCookiesEncryptionKey PowerShell 命令 chaged，此事件是正常的，且不需要任何解決步驟。|檢查與 AD FS 的連線能力。 您可以使用連結 HTTPs://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xml確定已在 AD FS 與 Web 應用程式 Proxy 之間建立信任。 如果這些解決方案無法運作，請執行 Install-WebApplicationProxy Cmdlet。|
|12008<p>Web 應用程式 Proxy 超過允許的後端伺服器的 Kerberos 驗證嘗試次數上限。|此事件可能表示 Web 應用程式 Proxy 與後端應用程式伺服器之間的設定不正確，或兩部電腦上的日期和時間設定有問題。|後端伺服器拒絕了 Web 應用程式 Proxy 所建立的 Kerberos 票證。 確認已正確設定 Web 應用程式 Proxy 和後端應用程式伺服器的設定。<p>確定 Web 應用程式 Proxy 和後端應用程式伺服器上的日期和時間設定已同步處理。|
|12011<p>Web 應用程式 Proxy 收到具有非有效存取 cookie 簽章的要求。|此事件可能表示 Web 應用程式 Proxy 和 AD FS 未連線，或是未收到相同的設定。 如果您執行 "AccessCookiesEncryptionKey" 參數是由 Set-WebApplicationProxyConfiguration-RegenerateAccessCookiesEncryptionKey PowerShell 命令 chaged，此事件是正常的，且不需要任何解決步驟。|檢查與 AD FS 的連線能力。 您可以使用連結 HTTPs://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xml確定已在 AD FS 與 Web 應用程式 Proxy 之間建立信任。 如果這些解決方案無法運作，請執行 Install-WebApplicationProxy Cmdlet。|
|12027<p>Proxy 在處理要求時發生未預期的錯誤。 提供的名稱不是正確格式的帳戶名稱。|此事件可能表示 Web 應用程式 Proxy 與域控制器伺服器之間的設定不正確，或兩部電腦上的日期和時間設定有問題。|網域控制站拒絕了 Web 應用程式 Proxy 所建立的 Kerberos 票證。 確認已正確設定 Web 應用程式 Proxy 和後端應用程式伺服器的設定，特別是 SPN 設定。 請確認 Web 應用程式 Proxy 已加入網網域控制站相同網域的網域，以確保網域控制站會與 Web 應用程式 Proxy 建立信任關係。請確認 Web 應用程式 Proxy 和網域控制站上的日期和時間設定已同步處理。|
|13012<p>Web 應用程式 Proxy 收到不正確 edge 權杖簽章||確定您已將 Web 應用程式 Proxy 更新為 [KB 2955164](https://go.microsoft.com/fwlink/?LinkId=400701)。|
|13013<p>Web 應用程式 Proxy 收到包含已過期 edge 權杖的要求。|Web 應用程式 Proxy 和 AD FS 沒有同步處理的時鐘。|將 Web 應用程式 Proxy 與 AD FS 之間的時鐘進行同步處理。|
|13014<p>Web 應用程式 Proxy 收到具有無效 edge 權杖的要求。 權杖無效，因為無法剖析。|這可能表示 AD FS 設定有問題。|檢查您的 AD FS 設定，並視需要還原預設設定。|
|13015<p>Web 應用程式 Proxy 收到具有到期存取 cookie 的要求。|這可能表示未同步處理的時鐘。|如果您使用的是 Web 應用程式 Proxy 電腦的叢集，請確定電腦的時間和日期已同步處理。|
|13016<p>因為 edge 權杖或存取 cookie 中沒有 UPN，所以 Web 應用程式 Proxy 無法代表使用者取得 Kerberos 票證。|STS 組態有問題。|在 STS 中修正 UPN 宣告設定。|
|13019<p>Web 應用程式 Proxy 因為下列一般 API 錯誤，所以無法代表使用者取得 Kerberos 票證|此事件可能表示 Web 應用程式 Proxy 與域控制器伺服器之間的設定不正確，或兩部電腦上的日期和時間設定有問題。|網域控制站拒絕了 Web 應用程式 Proxy 所建立的 Kerberos 票證。 確認已正確設定 Web 應用程式 Proxy 和後端應用程式伺服器的設定，特別是 SPN 設定。 請確認 Web 應用程式 Proxy 已加入網網域控制站相同網域的網域，以確保網域控制站會與 Web 應用程式 Proxy 建立信任關係。請確認 Web 應用程式 Proxy 和網域控制站上的日期和時間設定已同步處理。|
|13020<p>Web 應用程式 Proxy 無法代表使用者取得 Kerberos 票證，因為未定義後端伺服器 SPN。|此事件可能表示 Web 應用程式 Proxy 與域控制器伺服器之間的設定不正確，或兩部電腦上的日期和時間設定有問題。|網域控制站拒絕了 Web 應用程式 Proxy 所建立的 Kerberos 票證。 確認已正確設定 Web 應用程式 Proxy 和後端應用程式伺服器的設定，特別是 SPN 設定。 請確認 Web 應用程式 Proxy 已加入網網域控制站相同網域的網域，以確保網域控制站會與 Web 應用程式 Proxy 建立信任關係。請確認 Web 應用程式 Proxy 和網域控制站上的日期和時間設定已同步處理。|
|13022<p>Web 應用程式 Proxy 無法驗證使用者，因為後端伺服器以 HTTP 401 錯誤回應 Kerberos 驗證嘗試。|此事件可能表示 Web 應用程式 Proxy 與後端應用程式伺服器之間的設定不正確，或兩部電腦上的日期和時間設定有問題。|後端伺服器拒絕了 Web 應用程式 Proxy 所建立的 Kerberos 票證。 確認已正確設定 Web 應用程式 Proxy 和後端應用程式伺服器的設定。確定 Web 應用程式 Proxy 和後端應用程式伺服器上的日期和時間設定已同步處理。|
|13025<p>用戶端未向 Web 應用程式 Proxy 出示 SSL 憑證。|此事件可能表示時間和日期設定有問題。|請確定憑證基礎結構有效，且 Web 應用程式 Proxy 和 AD FS 的時間和日期已同步處理。 請確定為 Web 應用程式 Proxy 設定的指紋是正確的。|
|13026<p>用戶端向 Web 應用程式 Proxy 出示了 SSL 憑證，但憑證無效：憑證不符合指紋。|此事件可能表示時間和日期設定有問題。|請確定憑證基礎結構有效，且 Web 應用程式 Proxy 和 AD FS 的時間和日期已同步處理。 請確定為 Web 應用程式 Proxy 設定的指紋是正確的。|
|13028<p>Web 應用程式 Proxy 收到的要求包含尚未生效的 edge 權杖。|此事件可能表示時間和日期設定有問題。|請確定憑證基礎結構有效，且 Web 應用程式 Proxy 和 AD FS 的時間和日期已同步處理。|
|13030<p>用戶端會向 Web 應用程式 Proxy 出示 SSL 憑證，但信任提供者不會信任發出用戶端憑證的憑證授權單位單位。|此事件可能表示時間和日期設定有問題。|請確定憑證基礎結構有效，且 Web 應用程式 Proxy 和 AD FS 的時間和日期已同步處理。 請確定為 Web 應用程式 Proxy 設定的指紋是正確的。|
|13031<p>用戶端向 Web 應用程式 Proxy 出示 SSL 憑證，但憑證鏈在信任提供者不信任的根憑證中終止。|此事件可能表示時間和日期設定有問題。|請確定憑證基礎結構有效，且 Web 應用程式 Proxy 和 AD FS 的時間和日期已同步處理。 請確定為 Web 應用程式 Proxy 設定的指紋是正確的。|
|13032<p>用戶端向 Web 應用程式 Proxy 出示了 SSL 憑證，但憑證對要求的使用方式無效。|此事件可能表示時間和日期設定有問題。|請確定憑證基礎結構有效，且 Web 應用程式 Proxy 和 AD FS 的時間和日期已同步處理。 請確定為 Web 應用程式 Proxy 設定的指紋是正確的。|
|13033<p>用戶端會向 Web 應用程式 Proxy 出示 SSL 憑證，但憑證在驗證時，不在其有效期間內，而是在驗證已簽署檔案中的目前系統時鐘或時間戳記時。|此事件可能表示時間和日期設定有問題。|請確定憑證基礎結構有效，且 Web 應用程式 Proxy 和 AD FS 的時間和日期已同步處理。 請確定為 Web 應用程式 Proxy 設定的指紋是正確的。|
|13034<p>用戶端向 Web 應用程式 Proxy 出示了 SSL 憑證，但憑證無效。|此事件可能表示時間和日期設定有問題。|請確定憑證基礎結構有效，且 Web 應用程式 Proxy 和 AD FS 的時間和日期已同步處理。 請確定為 Web 應用程式 Proxy 設定的指紋是正確的。|

下列系統管理員主控台事件通常表示必須進行設定的問題，例如布建、要求不成功、無法連線的後端伺服器，以及緩衝區溢位。

|事件或徵兆|可能的原因|解決方案|
|--------------------|------------------|--------------|
|12019<p>Web 應用程式 Proxy 無法為下列 URL 建立接聽程式。|事件的可能原因是另一項服務正在接聽相同的 URL。|系統管理員必須確定沒有人接聽或系結至相同的 Url。 若要檢查這一點，請執行下列命令： netsh HTTP show urlacl。 如果在 Web 應用程式 Proxy 電腦上執行的另一個元件使用此 URL，請將它移除，或使用不同的 URL 透過 Web Application Proxy 發行應用程式。|
|12020<p>Web 應用程式 Proxy 無法建立下列 URL 的保留。|事件的可能原因是另一個服務在相同的 URL 上具有保留。|系統管理員必須確定沒有人系結至相同的 Url。 若要檢查這一點，請執行下列命令： netsh HTTP show urlacl。 如果在 Web 應用程式 Proxy 電腦上執行的另一個元件使用此 URL，請將它移除，或使用不同的 URL 透過 Web Application Proxy 發行應用程式。|
|12021<p>Web 應用程式 Proxy 無法系結 SSL 伺服器憑證。 已套用所有其他設定。|無法建立及設定 SSL 憑證資料的設定記錄。|確定為 Web 應用程式 Proxyapplications 設定的憑證指紋已安裝在本機電腦存放區中具有私密金鑰的所有 Web 應用程式 Proxy 電腦上。|
|13001<p>後端伺服器向 Web 應用程式 Proxy 呈現的 SSL 伺服器憑證無效;憑證不受信任。|伺服器傳送的安全通訊端層 (SSL) 憑證中找到一或多個錯誤。 這可能表示後端伺服器提供了不正確 SSL，或是 Web 應用程式 Proxy 與後端伺服器之間沒有信任關係。|驗證後端伺服器 SSL 憑證。 確定已使用正確的根 Ca 設定 Web 應用程式 Proxy 電腦，以信任後端伺服器憑證。|
|13006|0x80072ee7 錯誤碼時，failurrre 是因為無法解析後端伺服器 URL 所造成。 其他錯誤碼詳述于 [https://msdn.microsoft.com/library/windows/desktop/aa384110(v=vs.85)](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))|檢查後端伺服器 URL 是否正確，並可從 Web 應用程式 Proxy 電腦正確地解析其名稱。|
|13007<p>在預期的間隔內，未收到來自後端伺服器的 HTTP 回應。|後端伺服器要求超時或速度很慢或沒有回應。|檢查後端伺服器設定。 如果速度很慢，請檢查後端伺服器的連線能力，並考慮變更適用于 InactiveTransactionsTimeoutSec 的 Web 應用程式 Proxy 全域設定參數 Cmdlet。|

## <a name="see-also"></a>另請參閱
[Windows Server 2016 中 Web 應用程式 Proxy 的新功能](web-application-proxy-windows-server.md) 
使用[Web 應用程式 Proxy](assetId:///b607b717-2172-4271-98d1-fa8162e0bb2e)


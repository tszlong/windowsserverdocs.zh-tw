---
title: 對 Web 應用程式 Proxy 進行疑難排解
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a2fef55d-747b-4e20-8f21-5f8807e7ef87
ms.technology: web-app-proxy
ms.openlocfilehash: c63dbc415765cbe60bfbde7bf180f3dd967d6ab8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841579"
---
# <a name="troubleshooting-web-application-proxy"></a>對 Web 應用程式 Proxy 進行疑難排解

>適用於：Windows Server 2016

**此內容是關於 Web 應用程式 Proxy 的內部部署版本的項目。若要在雲端上啟用安全存取內部部署應用程式，請參閱[Azure AD Application Proxy 內容](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/)。**  
  
本節提供的疑難排解程序，包括事件的說明以及解決方案的 Web 應用程式 proxy。 有三個位置會顯示錯誤：  
  
-   在 Web 應用程式 Proxy 系統管理員主控台  
  
    系統管理員主控台中所列每個事件識別碼可以檢視 Windows 事件檢視器和對應的描述中下, 面找到解決方案。  
  
    開啟事件檢視器] 並尋找 Web 應用程式 Proxy 」 的 [與相關的事件**Applications and Services Logs** > **Microsoft** > **Windows**  >  **Web 應用程式 Proxy** > **系統管理員**  
  
    ![](media/Troubleshooting-Web-Application-Proxy/WebApplicationProxyTroubleshooting.png)  
  
    如有需要詳細的記錄檔都可以透過開啟分析和偵錯記錄檔，並在 Windows 事件檢視器 中開啟 Web 應用程式 Proxy 工作階段記錄檔中，找到 \ Microsoft \ Windows \ Web 應用程式 Proxy \ 系統管理員。  
  
-   PowerShell 錯誤  
  
    在設定期間遇到的問題的事件會顯示在 PowerShell 中。  
  
    PowerShell 使用者使用標準 PowerShell 錯誤提示，會顯示所有錯誤。 所有的 PowerShell 命令會記錄為事件。 在 PowerShell 中發生的所有事件的 ID 編號 12016，與 Windows 事件檢視器中所列及 PowerShell 一節中定義如下。  
  
-   在 Best Practices Analyzer  
  
    這些事件中所述[Web 應用程式 proxy 的最佳做法分析程式](assetId:///995f5cea-e334-4ce6-a14c-6a2d03c0c79a)  
  
## <a name="powershell-messages"></a>PowerShell 訊息  
  
|事件或徵狀|可能的原因|解析度|  
|--------------------|------------------|--------------|  
|信任的憑證 (「 ADFS ProxyTrust- <WAP machine name>") 無效|可能是由下列任何原因所造成：<br /><br />-應用程式 Proxy 電腦已關閉的時間太長。<br />-Web 應用程式 Proxy 與 AD FS 之間的連線中斷<br />憑證基礎結構問題<br />-變更 AD FS 的電腦，或 Web 應用程式 Proxy 與 AD FS 之間的更新程序未執行，規劃每隔 8 小時，則他們必須更新信任<br />-Web 應用程式 Proxy 電腦和 AD FS 的時鐘未同步處理。|請確定在時鐘。 執行 Install-webapplicationproxy cmdlet。|  
|在 AD FS 中找不到組態資料|這可能是因為 Web 應用程式 Proxy 未完整安裝還或因為資料庫損毀的 AD FS 資料庫中的變更。|執行 Install-webapplicationproxy Cmdlet|  
|Web 應用程式 Proxy 會嘗試從 AD FS 中讀取設定時發生錯誤。|這可能表示 AD FS 不可以連線，或 AD FS 發生嘗試讀取 AD FS 資料庫中的組態發生內部問題。|確認 AD FS 已連線且運作正常。|  
|儲存在 AD FS 組態資料已損毀或 Web 應用程式 Proxy 無法進行剖析。<br /><br />或<br /><br />Web 應用程式 Proxywas 無法擷取 AD FS 信賴憑證者的合作對象的清單。|如果組態資料已修改 AD FS 中，這可能會發生。|重新啟動 Web 應用程式 Proxyservice。 如果問題持續發生，請執行 Install-webapplicationproxy Cmdlet。|  
  
## <a name="administrator-console-events"></a>系統管理員主控台事件  
下列的系統管理員主控台事件通常表示發生驗證錯誤，tokes 無效或過期的 cookie。  
  
|事件或徵狀|可能的原因|解析度|  
|--------------------|------------------|--------------|  
|11005<br /><br />Web 應用程式 Proxy 無法建立 cookie 加密金鑰，使用組態中的密碼。|全域組態 」 AccessCookiesEncryptionKey 」 參數已變更的 PowerShell 命令：Set-WebApplicationProxyConfiguration -RegenerateAccessCookiesEncryptionKey|需要任何動作不。 有問題的 cookie 已移除，且使用者已重新導向至 STS 進行驗證。|  
|12000<br /><br />Web 應用程式 Proxy 無法檢查組態變更至少 60 分鐘|Web 應用程式 Proxy 無法存取使用此命令取得 WebApplicationProxyConfiguration/應用程式的 Web 應用程式 Proxy 設定。 這種情形通常因缺少與 AD FS 搭配所連線的問題或需要更新與 AD FS 的信任。|檢查與 AD FS 的連線。 您可以使用連結 https://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xmlMake 確定 AD FS 與 Web 應用程式 Proxy 之間建立信任關係。 如果這些解決方案沒有作用，請執行 Install-webapplicationproxy Cmdlet。|  
|12003<br /><br />Web 應用程式 Proxy 無法剖析存取 cookie。|這可能表示 Web 應用程式 Proxy 和 AD FS 未連線，或它們不會收到相同的組態。|檢查與 AD FS 的連線。 您可以使用連結 https://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xmlMake 確定 AD FS 與 Web 應用程式 Proxy 之間建立信任關係。 如果這些解決方案沒有作用，請執行 Install-webapplicationproxy Cmdlet。|  
|12004<br /><br />Web 應用程式 Proxy 收到無效的存取 cookie 的要求。|此事件可能表示 Web 應用程式 Proxy 和 AD FS 未連線，或它們不會收到相同的組態。<br /><br />如果您執行 「 AccessCookiesEncryptionKey 」 參數已 chaged 由 Set-webapplicationproxyconfiguration RegenerateAccessCookiesEncryptionKey PowerShell 命令，這個事件是正常現象，不需要任何解決步驟。|檢查與 AD FS 的連線。 您可以使用連結 https://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xmlMake 確定 AD FS 與 Web 應用程式 Proxy 之間建立信任關係。 如果這些解決方案沒有作用，請執行 Install-webapplicationproxy Cmdlet。|  
|12008<br /><br />Web 應用程式 Proxy 會超過允許的 Kerberos 驗證嘗試的後端伺服器的數目上限。|此事件可能表示 Web 應用程式 Proxy 與後端應用程式伺服器或在兩台電腦上的日期和時間設定問題之間的設定不正確。|後端伺服器拒絕了建立 Web 應用程式 Proxy 的 Kerberos 票證。 請確認已正確設定 Web 應用程式 Proxy 與後端應用程式伺服器的組態。<br /><br />請確定 Web 應用程式 Proxy 」 和 「 後端應用程式伺服器上的日期和時間設定已同步處理。|  
|12011<br /><br />Web 應用程式 Proxy 收到無效的存取 cookie 簽章的要求。|此事件可能表示 Web 應用程式 Proxy 和 AD FS 未連線，或它們不會收到相同的組態。 如果您執行 「 AccessCookiesEncryptionKey 」 參數已 chaged 由 Set-webapplicationproxyconfiguration RegenerateAccessCookiesEncryptionKey PowerShell 命令，這個事件是正常現象，不需要任何解決步驟。|檢查與 AD FS 的連線。 您可以使用連結 https://<FQDN_AD_FS_Proxy>/FederationMetadata/2007-06/FederationMetadata.xmlMake 確定 AD FS 與 Web 應用程式 Proxy 之間建立信任關係。 如果這些解決方案沒有作用，請執行 Install-webapplicationproxy Cmdlet。|  
|12027<br /><br />Proxy 會在處理要求時發生未預期的錯誤。 提供的名稱不是正確格式的帳戶名稱。|此事件可能表示 Web 應用程式 Proxy 與網域控制站伺服器或在兩台電腦上的日期和時間設定問題之間的設定不正確。|網域控制站會拒絕建立 Web 應用程式 Proxy 的 Kerberos 票證。 確認 Web 應用程式 Proxy 與後端應用程式伺服器的組態設定正確，尤其是 SPN 組態。 請確定 Web 應用程式 Proxy 已加入網域的網域控制站，以確保網域控制站會確定建立與 Web 應用程式 Proxy.Make 信任相同的網域，在 Web Application Proxy 上的日期和時間設定，網域控制站會同步處理。|  
|13012<br /><br />Web 應用程式 Proxy 收到無效的 edge 權杖簽章||請確定您更新具有 Web 應用程式 Proxy [KB 2955164](https://go.microsoft.com/fwlink/?LinkId=400701)。|  
|13013<br /><br />Web 應用程式 Proxy 會收到包含過期的 edge 權杖的要求。|Web 應用程式 Proxy 和 AD FS 不需要同步處理的時鐘。|同步處理 Web 應用程式 Proxy 與 AD FS 之間的時鐘。|  
|13014<br /><br />Web 應用程式 Proxy 收到無效的 edge 權杖的要求。 權杖不是有效的因為無法剖析。|這可能表示發生問題的 AD FS 設定。|檢查您的 AD FS 設定，如有必要，還原預設組態。|  
|13015<br /><br />Web 應用程式 Proxy 收到過期的存取 cookie 的要求。|這可能表示不同步的時鐘。|如果您正在使用 Web Application Proxy 電腦叢集，請確定機器的日期與時間已同步處理。|  
|13016<br /><br />Web 應用程式 Proxy 無法擷取代表使用者的 Kerberos 票證，因為 edge 權杖或存取 cookie 中沒有 upn。|沒有 STS 組態有問題。|在 STS 中修正 UPN 宣告設定。|  
|13019<br /><br />Web 應用程式 Proxy 無法擷取代表使用者的 Kerberos 票證，因為發生下列一般 API 錯誤|此事件可能表示 Web 應用程式 Proxy 與網域控制站伺服器或在兩台電腦上的日期和時間設定問題之間的設定不正確。|網域控制站會拒絕建立 Web 應用程式 Proxy 的 Kerberos 票證。 確認 Web 應用程式 Proxy 與後端應用程式伺服器的組態設定正確，尤其是 SPN 組態。 請確定 Web 應用程式 Proxy 已加入網域的網域控制站，以確保網域控制站會確定建立與 Web 應用程式 Proxy.Make 信任相同的網域，在 Web Application Proxy 上的日期和時間設定，網域控制站會同步處理。|  
|13020<br /><br />Web 應用程式 Proxy 無法擷取代表使用者的 Kerberos 票證，因為未定義後端伺服器 SPN。|此事件可能表示 Web 應用程式 Proxy 與網域控制站伺服器或在兩台電腦上的日期和時間設定問題之間的設定不正確。|網域控制站會拒絕建立 Web 應用程式 Proxy 的 Kerberos 票證。 確認 Web 應用程式 Proxy 與後端應用程式伺服器的組態設定正確，尤其是 SPN 組態。 請確定 Web 應用程式 Proxy 已加入網域的網域控制站，以確保網域控制站會確定建立與 Web 應用程式 Proxy.Make 信任相同的網域，在 Web Application Proxy 上的日期和時間設定，網域控制站會同步處理。|  
|13022<br /><br />Web 應用程式 Proxy 無法驗證使用者，因為後端伺服器回應 Kerberos 驗證嘗試，以 HTTP 401 錯誤。|此事件可能表示 Web 應用程式 Proxy 與後端應用程式伺服器或在兩台電腦上的日期和時間設定問題之間的設定不正確。|後端伺服器拒絕了建立 Web 應用程式 Proxy 的 Kerberos 票證。 請確認已正確設定 Web 應用程式 Proxy 與後端應用程式伺服器的組態。請確定 Web 應用程式 Proxy 」 和 「 後端應用程式伺服器上的日期和時間設定已同步處理。|  
|13025<br /><br />用戶端未顯示 Web 應用程式 proxy 的 SSL 憑證。|此事件可能表示日期和時間設定有問題。|請確定憑證基礎結構有效，且 Web 應用程式 Proxy 和 AD FS 的日期與時間會同步處理。 請確定針對 Web 應用程式 Proxy 設定的指紋正確。|  
|13026<br /><br />用戶端向 Web Application Proxy 的 SSL 憑證，但不是有效的憑證： 憑證不符合憑證指紋。|此事件可能表示日期和時間設定有問題。|請確定憑證基礎結構有效，且 Web 應用程式 Proxy 和 AD FS 的日期與時間會同步處理。 請確定針對 Web 應用程式 Proxy 設定的指紋正確。|  
|13028<br /><br />Web 應用程式 Proxy 會收到包含尚未生效的 edge 權杖的要求。|此事件可能表示日期和時間設定有問題。|請確定憑證基礎結構有效，且 Web 應用程式 Proxy 和 AD FS 的日期與時間會同步處理。|  
|13030<br /><br />用戶端向 Web Application Proxy 的 SSL 憑證，但信任提供者不信任憑證授權單位發出的用戶端憑證。|此事件可能表示日期和時間設定有問題。|請確定憑證基礎結構有效，且 Web 應用程式 Proxy 和 AD FS 的日期與時間會同步處理。 請確定針對 Web 應用程式 Proxy 設定的指紋正確。|  
|13031<br /><br />用戶端向 Web Application Proxy 的 SSL 憑證但它終止於信任提供者不信任的根憑證的憑證鏈結。|此事件可能表示日期和時間設定有問題。|請確定憑證基礎結構有效，且 Web 應用程式 Proxy 和 AD FS 的日期與時間會同步處理。 請確定針對 Web 應用程式 Proxy 設定的指紋正確。|  
|13032<br /><br />用戶端向 Web Application Proxy 的 SSL 憑證，但憑證要求的使用方式無效。|此事件可能表示日期和時間設定有問題。|請確定憑證基礎結構有效，且 Web 應用程式 Proxy 和 AD FS 的日期與時間會同步處理。 請確定針對 Web 應用程式 Proxy 設定的指紋正確。|  
|13033<br /><br />用戶端向 Web Application Proxy 的 SSL 憑證，但在其有效期間內沒有憑證時針對目前的系統時鐘或已簽署的檔案中的時間戳記進行驗證。|此事件可能表示日期和時間設定有問題。|請確定憑證基礎結構有效，且 Web 應用程式 Proxy 和 AD FS 的日期與時間會同步處理。 請確定針對 Web 應用程式 Proxy 設定的指紋正確。|  
|13034<br /><br />用戶端向 Web Application Proxy 的 SSL 憑證，但憑證無效。|此事件可能表示日期和時間設定有問題。|請確定憑證基礎結構有效，且 Web 應用程式 Proxy 和 AD FS 的日期與時間會同步處理。 請確定針對 Web 應用程式 Proxy 設定的指紋正確。|  
  
下列的系統管理員主控台事件通常表示需要進行設定，例如佈建、 不成功的要求、 後端伺服器無法連線到的問題，而且緩衝區溢位。  
  
|事件或徵狀|可能的原因|解析度|  
|--------------------|------------------|--------------|  
|12019<br /><br />Web 應用程式 Proxy 無法建立下列 URL 的接聽程式。|事件的原因可能是另一個服務用來接聽以相同的 URL。|系統管理員必須確定沒有人會接聽或繫結至相同的 Url。 若要檢查此情形，執行命令： netsh http show urlacl。 如果此 URL 由 Web 應用程式 Proxy 電腦上執行的另一個元件，將它移除，或是發行透過 Web Application Proxy 應用程式使用不同的 URL。|  
|12020<br /><br />Web 應用程式 Proxy 無法建立下列 url 保留項目。|事件的原因可能是另一個服務，具有相同的 URL 保留項目。|系統管理員必須先確定沒有人會繫結至相同的 Url。 若要檢查此情形，執行命令： netsh http show urlacl。 如果此 URL 由 Web 應用程式 Proxy 電腦上執行的另一個元件，將它移除，或是發行透過 Web Application Proxy 應用程式使用不同的 URL。|  
|12021<br /><br />Web 應用程式 Proxy 無法繫結的 SSL 伺服器憑證。 已套用所有其他組態設定。|無法建立和設定的 SSL 憑證資料的組態記錄。|請確定已針對 Web 應用程式 Proxyapplications 的憑證指紋，會使用私密金鑰在本機電腦存放區中所有的 Web 應用程式 Proxy 電腦上安裝。|  
|13001<br /><br />後端伺服器提供 Web 應用程式 proxy 的 SSL 伺服器憑證不正確;憑證不受信任。|找不到伺服器所傳送的 Secure Sockets Layer (SSL) 憑證中的一個或多個錯誤。 這可能表示後端伺服器提供不是有效的 SSL，或沒有 Web 應用程式 Proxy 與後端伺服器之間沒有信任關係。|驗證後端伺服器的 SSL 憑證。 請確定 Web 應用程式 Proxy 電腦已設定的正確根信任的後端伺服器憑證的 Ca。|  
|13006|0x80072ee7 錯誤程式碼時，failurrre 被因為無法解析後端伺服器 URL。 中所述的其他錯誤碼 [https://msdn.microsoft.com/library/windows/desktop/aa384110(v=vs.85)](https://msdn.microsoft.com/library/windows/desktop/aa384110(v=vs.85))|請檢查後端伺服器 URL 正確，且其名稱是可從 Web 應用程式 Proxy 電腦正確解析。|  
|13007<br /><br />預期的間隔時間內沒有收到來自後端伺服器之 HTTP 回應。|後端伺服器要求已逾時，或會變慢或沒有回應。|請檢查後端伺服器組態。 如果速度很慢，請檢查後端伺服器的連線，也請考慮變更 Web 應用程式 Proxy 的全域組態參數 cmdlet InactiveTransactionsTimeoutSec。|  
  
## <a name="see-also"></a>另請參閱  
[什麼是 Windows Server 2016 中的 Web 應用程式 Proxy 的新功能](web-application-proxy-windows-server.md)  
[使用 Web 應用程式 Proxy](assetId:///b607b717-2172-4271-98d1-fa8162e0bb2e)  
  



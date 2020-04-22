---
title: Windows Server 中 DNS 伺服器的新功能
description: 本主題概要說明 Windows Server 2016 和更新版本中 DNS 伺服器的新功能
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 26d9a532f8c2276a81e8718e76290d41c78f6633
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317969"
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>Windows Server 中 DNS 伺服器的新功能

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題說明 Windows Server 2016 中新增或變更的網域名稱系統（DNS）伺服器功能。  
  
在 Windows Server 2016 中，DNS 伺服器提供下列方面的增強支援。  
  
|功能|新功能或增強功能|描述|  
|-----------------|-------------------|---------------|  
|DNS 原則|新增|您可以設定 DNS 原則，以指定 DNS 伺服器回應 DNS 查詢的方式。 DNS 回應可以根據用戶端 IP 位址（位置）、一天的時間，以及數個其他參數。 DNS 原則可啟用位置感知的 DNS、流量管理、負載平衡、分裂式 DNS 和其他案例。|  
|回應速率限制（RRL）|新增|您可以在 DNS 伺服器上啟用回應速率限制。 如此一來，您就可以避免惡意系統使用您的 DNS 伺服器在 DNS 用戶端上起始阻絕服務攻擊的可能性。|  
|命名實體的 DNS 驗證（來）|新增|您可以使用 TLSA （傳輸層安全性驗證）記錄來提供資訊給 DNS 用戶端，以指出他們應從您的功能變數名稱取得憑證的 CA。 這可防止中間人攻擊，其中有人可能會損毀 DNS 快取以指向自己的網站，並提供他們從不同 CA 發行的憑證。|  
|未知的記錄支援|新增|您可以使用「未知記錄」功能來新增 Windows DNS 伺服器不明確支援的記錄。|  
|IPv6 根目錄提示|新增|您可以使用原生 IPV6 根提示支援，使用 IPV6 根功能變數名稱伺服器來執行網際網路名稱解析。|  
|Windows PowerShell 支援|增強功能|新的 Windows PowerShell Cmdlet 適用于 DNS 伺服器。|  
  
## <a name="dns-policies"></a>DNS 原則

您可以將 DNS 原則用於地理位置型流量管理、以一天的時間為基礎的智慧型 DNS 回應、管理設定為分割\-大腦部署的單一 DNS 伺服器、在 DNS 查詢上套用篩選等等。 下列專案提供有關這些功能的詳細資料。

-   **應用程式負載平衡。** 當您已在不同位置部署應用程式的多個實例時，可以使用 DNS 原則來平衡不同應用程式實例之間的流量負載，以動態方式配置應用程式的流量負載。

-   **異地\-位置為基礎的流量管理。** 您可以使用 DNS 原則，允許主要和次要 DNS 伺服器根據用戶端和用戶端嘗試連線之資源的地理位置來回應 DNS 用戶端查詢，為用戶端提供最接近的 IP 位址resource. 

-   **分割大腦 DNS。** 使用 split\-大腦 DNS 時，DNS 記錄會分割成相同 DNS 伺服器上的不同區域範圍，而且 DNS 用戶端會根據用戶端是內部或外部用戶端接收回應。 您可以針對 Active Directory 整合區域或獨立 DNS 伺服器上的區域，設定分割\-大腦 DNS。

-   **濾除.** 您可以設定 DNS 原則，根據您提供的準則建立查詢篩選器。 DNS 原則中的查詢篩選器可讓您設定 DNS 伺服器，以自訂的方式，根據傳送 DNS 查詢的 DNS 查詢和 DNS 用戶端進行回應。 
-   **鑒識.** 您可以使用 DNS 原則，將惡意的 DNS 用戶端重新導向至不\-的現有 IP 位址，而不是將它們引導至其嘗試連線的電腦。

-   **以天為基礎的重新導向時間。** 您可以使用 DNS 原則，利用以當天時間為基礎的 DNS 原則，將應用程式流量分散到應用程式的不同地理位置上。 
  
您也可以將 DNS 原則用於 Active Directory 整合的 DNS 區域。

如需詳細資訊，請參閱[DNS 原則案例指南](deploy/DNS-Policies-Overview.md)。

## <a name="response-rate-limiting"></a>回應速率限制

您可以設定 RRL 設定，以控制當您的伺服器收到以相同用戶端為目標的數個要求時，如何回應 DNS 用戶端的要求。 如此一來，您就可以防止某人使用您的 DNS 伺服器傳送拒絕服務（Dos）攻擊。 例如，bot net 可以使用第三部電腦的 IP 位址做為要求者，將要求傳送至您的 DNS 伺服器。 如果沒有 RRL，您的 DNS 伺服器可能會回應所有的要求，並淹沒第三部電腦。 當您使用 [RRL] 時，您可以設定下列設定：  
  
-   **每秒的回應數**。 這是在一秒內將同一個回應提供給用戶端的次數上限。  
  
-   **每秒錯誤數**。 這是錯誤回應在一秒內傳送至相同用戶端的最大次數。  
  
-   **視窗**。 這是在提出太多要求時，對用戶端的回應將會暫停的秒數。  
  
-   流失**率**。 這是在暫停回應期間，DNS 伺服器回應查詢的頻率。 例如，如果伺服器暫停對用戶端的回應10秒，而流失率為5，則伺服器仍然會針對每5個傳送的查詢回應一個查詢。 這可讓合法用戶端取得回應，即使 DNS 伺服器在其子網或 FQDN 上套用回應速率限制也一樣。  
  
-   **TC 速率**。 這是用來通知用戶端在對用戶端的回應被擱置時，嘗試與 TCP 連線。 例如，如果 TC 速率為3，而伺服器暫停對指定用戶端的回應，則伺服器會針對每3個收到的查詢發出 TCP 連線的要求。 請確定 [TC 速率] 的值低於 [流失率]，讓用戶端選擇在洩漏回應之前透過 TCP 連接。  
  
-   **回應的最大值**。 這是當回應暫停時，伺服器會向用戶端發出的回應數目上限。  
  
-   **白名單網域**。 這是要從 RRL 設定中排除的網域清單。  
  
-   **允許清單子網**。 這是要從 RRL 設定中排除的子網清單。  
  
-   **白色清單伺服器介面**。 這是要從 RRL 設定中排除的 DNS 伺服器介面清單。  
  
## <a name="dane-support"></a>來支援

您可以使用來支援 \(RFC 6394 和 6698\) 來指定您的 DNS 用戶端，應將憑證發佈到您的 DNS 伺服器所裝載之網功能變數名稱稱的 CA。 這可避免某種形式的攔截式攻擊，讓某人能夠損毀 DNS 快取，並將 DNS 名稱指向自己的 IP 位址。  
  
例如，假設您使用名為 CA1 的知名授權單位所提供的憑證，在 www.contoso.com 上裝載使用 SSL 的安全網站。 有些人仍然可以從名為 CA2 的不同、不知名的憑證授權單位單位取得 www.contoso.com 的憑證。 然後，裝載假 www.contoso.com 網站的實體可能會損毀用戶端或伺服器的 DNS 快取，以將 www.contoto.com 指向其假的網站。 終端使用者將會看到來自 CA2 的憑證，而且可能只是確認它並聯機到假的網站。 透過來，用戶端會對 DNS 伺服器提出要求，以供 contoso.com 要求 TLSA 記錄，並瞭解 www.contoso.com 的憑證是否由 CA1 發出。 如果與另一個 CA 的憑證一起顯示，則會中止連線。  
  
## <a name="unknown-record-support"></a>未知的記錄支援

「未知的記錄」是 DNS 伺服器不知道其 .RDATA 格式的 RR。 新加入的「未知記錄」（RFC 3597）類型支援表示您可以將不支援的記錄類型新增到 Windows DNS 伺服器區域中的二進位連線格式。 Windows caching 解析程式已經有處理未知記錄類型的能力。 Windows DNS 伺服器不會針對未知的記錄執行任何記錄特定的處理，但會在收到查詢時傳迴響應。  
  
## <a name="ipv6-root-hints"></a>IPv6 根目錄提示

由 IANA 發佈的 IPV6 根提示已新增至 windows DNS 伺服器。 網際網路名稱查詢現在可以使用 IPv6 根功能變數名稱伺服器來執行名稱解析。

## <a name="windows-powershell-support"></a>Windows PowerShell 支援

Windows Server 2016 中引進了下列新的 Windows PowerShell Cmdlet 和參數。
  
-   **新增-DnsServerRecursionScope**。 此 Cmdlet 會在 DNS 伺服器上建立新的遞迴範圍。 DNS 原則會使用遞迴範圍來指定要在 DNS 查詢中使用的轉寄站清單。  
  
-   **移除-DnsServerRecursionScope**。 此 Cmdlet 會移除現有的遞迴範圍。  
  
-   **DnsServerRecursionScope**。 此 Cmdlet 會變更現有遞迴範圍的設定。  
  
-   **DnsServerRecursionScope**。 此 Cmdlet 會抓取現有遞迴範圍的相關資訊。  
  
-   **新增-DnsServerClientSubnet**。 此 Cmdlet 會建立新的 DNS 用戶端子網。 DNS 原則會使用子網來識別 DNS 用戶端的所在位置。  
  
-   **移除-DnsServerClientSubnet**。 此 Cmdlet 會移除現有的 DNS 用戶端子網。  
  
-   **DnsServerClientSubnet**。 此 Cmdlet 會變更現有 DNS 用戶端子網的設定。  
  
-   **DnsServerClientSubnet**。 此 Cmdlet 會抓取現有 DNS 用戶端子網的相關資訊。  
  
-   **新增-DnsServerQueryResolutionPolicy**。 此 Cmdlet 會建立新的 DNS 查詢解決原則。 DNS 查詢解析原則是用來根據不同的準則，指定查詢的回應方式。  
  
-   **移除-DnsServerQueryResolutionPolicy**。 此 Cmdlet 會移除現有的 DNS 原則。  
  
-   **DnsServerQueryResolutionPolicy**。 此 Cmdlet 會變更現有 DNS 原則的設定。  
  
-   **DnsServerQueryResolutionPolicy**。 此 Cmdlet 會抓取現有 DNS 原則的相關資訊。  
  
-   **啟用-DnsServerPolicy**。 此 Cmdlet 會啟用現有的 DNS 原則。  
  
-   **停用-DnsServerPolicy**。 此 Cmdlet 會停用現有的 DNS 原則。  
  
-   **新增-DnsServerZoneTransferPolicy**。 此 Cmdlet 會建立新的 DNS 伺服器區域傳輸原則。 DNS 區域轉送原則會指定要根據不同的準則來拒絕或忽略區域轉送。  
  
-   **移除-DnsServerZoneTransferPolicy**。 此 Cmdlet 會移除現有的 DNS 伺服器區域轉送原則。  
  
-   **DnsServerZoneTransferPolicy**。 此 Cmdlet 會變更現有 DNS 伺服器區域轉送原則的設定。  
  
-   **DnsServerResponseRateLimiting**。 此 Cmdlet 會抓取 RRL 設定。  
  
-   **DnsServerResponseRateLimiting**。 此 Cmdlet 會變更 RRL 設定也。  
  
-   **新增-DnsServerResponseRateLimitingExceptionlist**。 此 Cmdlet 會在 DNS 伺服器上建立一個 RRL 例外清單。  
  
-   **DnsServerResponseRateLimitingExceptionlist**。 此 Cmdlet 會抓取 RRL excception 清單。  
  
-   **移除-DnsServerResponseRateLimitingExceptionlist**。 此 Cmdlet 會移除現有的 RRL 例外狀況清單。  
  
-   **DnsServerResponseRateLimitingExceptionlist**。 此 Cmdlet 會變更 RRL 例外狀況清單。  
  
-   **新增-DnsServerResourceRecord**。 此 Cmdlet 已更新，以支援未知的記錄類型。  
  
-   **DnsServerResourceRecord**。 此 Cmdlet 已更新，以支援未知的記錄類型。  
  
-   **移除-DnsServerResourceRecord**。 此 Cmdlet 已更新，以支援未知的記錄類型。  
  
-   **DnsServerResourceRecord**。 此 Cmdlet 已更新，以支援未知的記錄類型

如需詳細資訊，請參閱下列 Windows Server 2016 Windows PowerShell 命令參考主題。

- [DnsServer 模組](https://docs.microsoft.com/powershell/module/dnsserver/?view=win10-ps)
- [Set-dnsclient 模組](https://docs.microsoft.com/powershell/module/dnsclient/?view=win10-ps)

## <a name="see-also"></a>另請參閱  
  
-   [DNS 用戶端的新功能](What-s-New-in-DNS-Client.md)  
  

  


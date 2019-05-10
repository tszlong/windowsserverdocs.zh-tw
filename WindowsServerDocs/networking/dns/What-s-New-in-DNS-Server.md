---
title: 什麼是 Windows Server 中的 DNS 伺服器的新功能
description: 本主題提供在 Windows Server 2016 和更新版本中的 DNS 伺服器的新功能的概觀
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 665e411eda834a59c6dbe3581611b9b58bd006f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833569"
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>什麼是 Windows Server 中的 DNS 伺服器的新功能

>適用於：Windows Server （半年通道），Windows Server 2016

本主題說明 Windows Server 2016 中新增或變更的網域名稱系統 (DNS) 伺服器功能。  
  
在 Windows Server 2016 中，DNS 伺服器會提供下列方面的增強的支援。  
  
|功能|新功能或增強功能|描述|  
|-----------------|-------------------|---------------|  
|DNS 原則|新的|您可以設定 DNS 原則，以指定的 DNS 伺服器回應 DNS 查詢的方式。 DNS 回應可以根據用戶端 IP 位址 （位置），時間的日期和數個其他參數。 DNS 原則可讓位置感知的 DNS、 流量管理、 負載平衡、 拆分式 DNS，以及其他案例。|  
|回應速率限制 (RRL)|新的|您可以讓您的 DNS 伺服器上的回應速率限制。 如此一來，您可以避免起始阻絕服務攻擊，DNS 用戶端上使用您的 DNS 伺服器的惡意系統的可能性。|  
|具名實體 （圖表） 的 DNS 為基礎的驗證|新的|您可以使用 TLSA （傳輸層安全性驗證） 的記錄資訊提供給狀態應該預期您的網域名稱的憑證從哪些 CA 的 DNS 用戶端。 這可防止有人可能會在此設定損毀的 DNS 快取，以指向自己的網站，並提供他們從不同的 CA 所發出之憑證的攔截層攻擊。|  
|未知的記錄支援|新的|您可以將明確使用未知的記錄功能的 Windows DNS 伺服器不支援記錄。|  
|IPv6 根提示|新的|您可以使用根提示來執行使用 IPV6 根伺服器的網際網路名稱解析所支援的原生 IPV6。|  
|Windows PowerShell Support|增強功能|新的 Windows PowerShell cmdlet 可供 DNS 伺服器。|  
  
## <a name="dns-policies"></a>DNS 原則

對於地理位置為基礎的流量管理，根據的時間，以管理單一的 DNS 伺服器設定為分割的智慧型 DNS 回應，您可以使用 DNS 原則\-大腦部署，DNS 查詢和多個套用的篩選。 下列項目會提供有關這些功能的更多詳細資料。

-   **應用程式負載平衡。** 當您部署的不同位置的應用程式的多個執行個體時，您可以使用 DNS 原則來平衡不同的應用程式執行個體，以動態方式配置應用程式的流量負載之間的流量負載。

-   **異地\-依據流量管理的位置。** 您可以使用 DNS 原則以允許主要和次要 DNS 伺服器回應 DNS 用戶端查詢，根據地理位置的用戶端和資源的用戶端嘗試連線，用戶端提供的最接近的 IP 位址資源。 

-   **分割 DNS 的大腦。** 分割\-大腦 DNS，DNS 記錄會被分割成不同的區域範圍，在相同的 DNS 伺服器和 DNS 用戶端收到回應，根據用戶端是否為內部或外部的用戶端。 您可以設定分割\-大腦 DNS 的 Active Directory 整合區域，或在獨立 DNS 伺服器上的區域。

-   **篩選。** 您可以設定 DNS 原則來建立查詢篩選器會根據您提供的準則。 DNS 原則中的查詢篩選器可讓您設定 DNS 伺服器回應 DNS 查詢和傳送 DNS 查詢的 DNS 用戶端為基礎的方式自訂。 
-   **鑑識調查。** 您可以使用 DNS 原則以惡意的 DNS 用戶端重新導向到非\-現存的 IP 位址，而不是將他們引導至他們嘗試連線到電腦。

-   **一天的時間基礎重新導向。** 您可以使用 DNS 原則以分散不同地理位置分散的應用程式執行個體中的應用程式流量，藉由使用 DNS 原則為基礎的當日時間。 
  
您也可以使用 DNS 原則，Active Directory 整合 DNS 區域。

如需詳細資訊，請參閱 < [DNS 原則案例指南](deploy/DNS-Policies-Overview.md)。

## <a name="response-rate-limiting"></a>回應速率限制

您可以設定 RRL 設定來控制如何回應 DNS 用戶端的要求，當您的伺服器接收數個以相同的用戶端為目標的要求。 如此一來，您可以防止其他人傳送使用您的 DNS 伺服器的阻絕服務 (Dos) 攻擊。 比方說，bot net 可以將要求傳送至您為要求者使用的第三個電腦的 IP 位址的 DNS 伺服器。 RRL，沒有您的 DNS 伺服器可能回應的所有要求，資料大量湧入的第三部電腦。 當您使用 RRL 時，您可以設定下列設定：  
  
-   **每秒的回應**。 這是相同的回應會提供給用戶端在一秒內最大次數。  
  
-   **每秒錯誤**。 這是錯誤回應會傳送至相同的用戶端在一秒內最大次數。  
  
-   **視窗**。 這是讓用戶端的回應即會暫止如果進行太多要求的秒數。  
  
-   **流失率**。 這是 DNS 伺服器回應就會暫停的時間內查詢回應的頻率。 比方說，如果伺服器會回應至用戶端暫停 10 秒，而流失率為 5，伺服器仍會回應來傳送每 5 個查詢的一個查詢。 這可讓合法的用戶端收到回應，即使在 DNS 伺服器會套用限制子網路或 FQDN 的回應率。  
  
-   **TC 速率**。 這用來告訴用戶端將嘗試回應給用戶端會暫止時，使用 TCP 連接。 比方說，如果 TC 速率為 3，且伺服器會暫停指定的用戶端的回應，伺服器會發出要求的每個收到的 3 個查詢的 TCP 連線。 請確定 TC 速率的值低於流失率，讓用戶端透過 TCP 連接之前流失回應的選項。  
  
-   **最大回應**。 這是的回應，回應會暫停執行時，伺服器將會對用戶端發出的最大數目。  
  
-   **允許清單的網域**。 這是要從 RRL 設定排除的網域清單。  
  
-   **允許清單的子網路**。 這是要從 RRL 設定排除的子網路的清單。  
  
-   **允許清單 server 介面**。 這是一份要排除 RRL 設定的 DNS 伺服器介面。  
  
## <a name="dane-support"></a>圖表支援

您可以使用 圖表支援\(RFC 6394 和 6698\)若要指定給您的 DNS 用戶端應該預期要從發行的網域名稱的憑證哪些 CA 裝載在您的 DNS 伺服器。 這可防止有人是能夠損毀的 DNS 快取，並指向其本身的 IP 位址的 DNS 名稱的攔截層攻擊形式。  
  
例如，假設您主機的安全的網站，在 www.contoso.com 中使用 SSL，方法是使用名為 CA1 已知的授權單位的憑證。 有人可能仍然可以從不同，不讓-鮮為人知，憑證授權單位，名為 「 CA2 www.contoso.com 取得憑證。 然後，實體裝載假 www.contoso.com 的網站可能能夠 www.contoto.com 指向其假的站台的用戶端或伺服器的 DNS 快取會損毀。 終端使用者將會看到憑證從 CA2，和可能只需確認它並連接到假的網站。 與圖表，會對要求 TLSA 記錄 contoso.com 的 DNS 伺服器提出要求的用戶端，並將其了解 www.contoso.com 的憑證已由 CA1 問題中。 如果看到從另一個 CA 憑證，則會中止連線。  
  
## <a name="unknown-record-support"></a>未知的記錄支援

「 未知的記錄 」 是 DNS 伺服器並不知道其 RDATA 格式 RR。 未知的記錄 (RFC 3597) 類型的新加入的支援會表示成 Windows DNS 伺服器區域，格式為二進位的連線，您可以新增不受支援的記錄類型。 Windows 快取解析程式已經能夠處理未知的記錄類型。 Windows DNS 伺服器將不會做任何記錄的特定處理未知的記錄，但會將它傳送回在回應中收到查詢，如果。  
  
## <a name="ipv6-root-hints"></a>IPv6 根提示

IPV6 的根提示，IANA，所發行之 windows DNS 伺服器已新增。 網際網路名稱查詢現在可以使用 IPv6 根伺服器執行名稱解析。

## <a name="windows-powershell-support"></a>Windows PowerShell 支援

Windows Server 2016 中引進下列新的 Windows PowerShell cmdlet 和參數。
  
-   **新增 DnsServerRecursionScope**。 此 cmdlet 會在 DNS 伺服器上建立新的遞迴範圍。 遞迴範圍可由 DNS 原則指定的 DNS 查詢中使用的轉寄站清單。  
  
-   **移除 DnsServerRecursionScope**。 此 cmdlet 會移除現有的遞迴領域。  
  
-   **設定 DnsServerRecursionScope**。 此 cmdlet 會變更現有的遞迴功能範圍的設定。  
  
-   **取得 DnsServerRecursionScope**。 此 cmdlet 會擷取現有的遞迴領域的相關資訊。  
  
-   **新增 DnsServerClientSubnet**。 此 cmdlet 會建立一個新的 DNS 用戶端子網路。 子網路用於 DNS 原則所識別的 DNS 用戶端所在的位置。  
  
-   **Remove-DnsServerClientSubnet**. 此 cmdlet 可移除現有的 DNS 用戶端子網路。  
  
-   **Set-DnsServerClientSubnet**. 此 cmdlet 會變更現有的 DNS 用戶端子網路的設定。  
  
-   **Get-DnsServerClientSubnet**. 此 cmdlet 會擷取現有的 DNS 用戶端子網路的相關資訊。  
  
-   **新增 DnsServerQueryResolutionPolicy**。 此 cmdlet 會建立新的 DNS 查詢解析原則。 DNS 查詢解析原則用來指定如何執行，或如果回應是查詢，根據不同的準則。  
  
-   **移除 DnsServerQueryResolutionPolicy**。 此 cmdlet 會移除現有的 DNS 原則。  
  
-   **設定 DnsServerQueryResolutionPolicy**。 此 cmdlet 會變更現有的 DNS 原則的設定。  
  
-   **取得 DnsServerQueryResolutionPolicy**。 此 cmdlet 會擷取現有的 DNS 原則的相關資訊。  
  
-   **啟用 DnsServerPolicy**。 此 cmdlet 可讓現有的 DNS 原則。  
  
-   **停用 DnsServerPolicy**。 此 cmdlet 會停用現有的 DNS 原則。  
  
-   **新增 DnsServerZoneTransferPolicy**。 此 cmdlet 會建立新的 DNS 伺服器區域傳輸原則。 DNS 區域傳輸原則會指定是否要拒絕或忽略根據不同準則區域轉送。  
  
-   **移除 DnsServerZoneTransferPolicy**。 此 cmdlet 會移除現有的 DNS 伺服器區域傳輸原則。  
  
-   **設定 DnsServerZoneTransferPolicy**。 此 cmdlet 會變更現有的 DNS 伺服器區域傳輸原則的設定。  
  
-   **取得 DnsServerResponseRateLimiting**。 此 cmdlet 會擷取 RRL 設定。  
  
-   **設定 DnsServerResponseRateLimiting**。 此 cmdlet 會變更 RRL 設定也。  
  
-   **新增 DnsServerResponseRateLimitingExceptionlist**。 此 cmdlet 會建立 RRL 例外狀況清單，DNS 伺服器上。  
  
-   **取得 DnsServerResponseRateLimitingExceptionlist**。 此 cmdlet 可擷取 RRL excception 清單。  
  
-   **移除 DnsServerResponseRateLimitingExceptionlist**。 此 cmdlet 可移除現有 RRL 例外狀況清單。  
  
-   **設定 DnsServerResponseRateLimitingExceptionlist**。 此 cmdlet 會變更 RRL 例外狀況清單。  
  
-   **新增 DnsServerResourceRecord**。 此 cmdlet 已更新為支援未知的記錄類型。  
  
-   **取得 DnsServerResourceRecord**。 此 cmdlet 已更新為支援未知的記錄類型。  
  
-   **移除 DnsServerResourceRecord**。 此 cmdlet 已更新為支援未知的記錄類型。  
  
-   **設定 DnsServerResourceRecord**。 此 cmdlet 已更新為支援未知的記錄類型

如需詳細資訊，請參閱下列的 Windows Server 2016 的 Windows PowerShell 命令參考主題。

- [DnsServer 模組](https://docs.microsoft.com/powershell/module/dnsserver/?view=win10-ps)
- [DnsClient 模組](https://docs.microsoft.com/powershell/module/dnsclient/?view=win10-ps)

## <a name="see-also"></a>另請參閱  
  
-   [什麼是 DNS 用戶端的新功能](What-s-New-in-DNS-Client.md)  
  

  


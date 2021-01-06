---
title: Windows Server 中 DNS 伺服器的新功能
description: 本主題概要說明 Windows Server 2016 和更新版本中 DNS 伺服器的新功能
manager: brianlic
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: lizross
author: eross-msft
ms.date: 12/18/2020
ms.openlocfilehash: ccbce13be36e6f4ff673fec27f25df1e653953bb
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949484"
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>Windows Server 中 DNS 伺服器的新功能

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明 Windows Server 2016 中新增或變更的網域名稱系統 (DNS) 伺服器功能。

在 Windows Server 2016 中，DNS 伺服器在下欄區域提供了增強的支援。

|功能|新功能或增強功能|描述|
|-----------------|-------------------|---------------|
|DNS 原則|新增|您可以設定 DNS 原則，以指定 DNS 伺服器回應 DNS 查詢的方式。 DNS 回應可以根據用戶端 IP 位址 (位置) 、一天中的時間，以及數個其他參數。 DNS 原則會啟用位置感知 DNS、流量管理、負載平衡、分割腦 DNS 和其他案例。|
|回應速率限制 (的 RRL) |新增|您可以在 DNS 伺服器上啟用回應速率限制。 如此一來，您就可以避免惡意系統使用您的 DNS 伺服器在 DNS 用戶端上起始阻絕服務攻擊的可能性。|
|命名實體的 DNS 型驗證 (來) |新增|您可以使用 TLSA (傳輸層安全性驗證) 記錄，將資訊提供給 DNS 用戶端，這些用戶端會陳述您的功能變數名稱應預期憑證的 CA。 這可防止有人可能損毀 DNS 快取以指向自己網站的攔截式攻擊，並提供從不同 CA 發行的憑證。|
|未知的記錄支援|新增|您可以使用未知的記錄功能來新增 Windows DNS 伺服器未明確支援的記錄。|
|IPv6 根目錄提示|新增|您可以使用原生 IPV6 根提示支援，利用 IPV6 根功能變數名稱伺服器來執行網際網路名稱解析。|
|Windows PowerShell 支援|改善|新的 Windows PowerShell Cmdlet 適用于 DNS 伺服器。|

## <a name="dns-policies"></a>DNS 原則

您可以使用 DNS 原則進行以 Geo-Location 為基礎的流量管理、以每天時間為基礎的智慧型 DNS 回應、管理針對分割腦部署設定的單一 DNS 伺服器 \- 、對 DNS 查詢套用篩選等。 下列專案提供這些功能的更多詳細資料。

-   **應用程式負載平衡。** 當您在不同位置部署應用程式的多個實例時，您可以使用 DNS 原則來平衡不同應用程式實例之間的流量負載，並動態配置應用程式的流量負載。

-   **以地理 \- 位置為基礎的流量管理。** 您可以使用 DNS 原則，根據用戶端嘗試連線的用戶端和資源的地理位置，讓主要和次要 DNS 伺服器回應 DNS 用戶端查詢，為用戶端提供最接近資源的 IP 位址。

-   **分割大腦 DNS。** 使用分割 \- 大腦 dns 時，dns 記錄會分割成相同 DNS 伺服器上的不同區域範圍，而 dns 用戶端會根據用戶端是內部或外部用戶端來接收回應。 您可以 \- 針對 Active Directory 整合式區域或獨立 DNS 伺服器上的區域，設定分割的大腦 DNS。

-   **濾波。** 您可以設定 DNS 原則，以根據您提供的準則建立查詢篩選準則。 DNS 原則中的查詢篩選器可讓您根據傳送 DNS 查詢的 DNS 查詢和 DNS 用戶端，將 DNS 伺服器設定為以自訂的方式回應。
-   **取證。** 您可以使用 DNS 原則，將惡意的 DNS 用戶端重新導向至不 \- 存在的 IP 位址，而不是將它們導向至它們嘗試連接的電腦。

-   **以天為基礎的重新導向時間。** 您可以使用 DNS 原則，藉由使用以一天時間為基礎的 DNS 原則，將應用程式流量分散到應用程式的不同地理位置分散的實例上。

您也可以使用 DNS 原則來 Active Directory 整合式 DNS 區域。

如需詳細資訊，請參閱 [DNS 原則案例指南](deploy/DNS-Policies-Overview.md)。

## <a name="response-rate-limiting"></a>回應速率限制

您可以設定 RRL 設定，以控制當您的伺服器收到數個以相同用戶端為目標的要求時，如何回應 DNS 用戶端的要求。 如此一來，您就可以防止某人使用您的 DNS 伺服器來傳送拒絕服務 (Dos) 攻擊。 比方說，bot net 可以使用第三部電腦的 IP 位址做為要求者，將要求傳送至您的 DNS 伺服器。 如果沒有 RRL，您的 DNS 伺服器可能會回應所有的要求，並淹沒第三台電腦。 使用 RRL 時，您可以設定下列設定：

-   **每秒的回應數**。 這是在一秒內提供給用戶端的相同回應的最大次數。

-   **每秒的錯誤數**。 這是在一秒內將錯誤回應傳送至相同用戶端的次數上限。

-   **視窗**。 這是在發出太多要求時，對用戶端的回應將暫停的秒數。

-   流失 **率**。 這是 DNS 伺服器在暫停回應時回應查詢的頻率。 比方說，如果伺服器在10秒內暫停對用戶端的回應，而且流失率為5，則伺服器仍會針對每個傳送的5個查詢回應一個查詢。 這可讓合法用戶端取得回應，即使 DNS 伺服器在其子網或 FQDN 上套用回應速率限制也一樣。

-   **TC 費率**。 這是用來告知用戶端在對用戶端的回應遭到擱置時，嘗試使用 TCP 進行連接。 比方說，如果 TC 速率是3，而伺服器暫停對指定用戶端的回應，則伺服器將會針對每3個收到的查詢發出 TCP 連接要求。 請確定 TC 速率的值低於流失率，以提供用戶端在洩漏回應之前透過 TCP 連接的選項。

-   **最大回應**。 這是伺服器在回應暫停時，將發出給用戶端的回應數目上限。

-   **允許清單網域**。 這是要從 RRL 設定排除的網域清單。

-   **允許清單子網**。 這是要從 RRL 設定排除的子網清單。

-   **允許清單伺服器介面**。 這是要從 RRL 設定排除的 DNS 伺服器介面清單。

## <a name="dane-support"></a>來支援

您可以使用來支援 \( RFC 6394 和 6698 \) ，針對 dns 伺服器所裝載的網功能變數名稱稱，指定您的 dns 用戶端應預期發出憑證的 CA。 這可防止某一種攔截式攻擊，也就是有人可以損毀 DNS 快取，並將 DNS 名稱指向自己的 IP 位址。

比方說，假設您使用名為 CA1 的知名授權單位中的憑證，裝載了在 www.contoso.com 使用 SSL 的安全網站。 有些人可能仍能從不同但不知名的憑證授權單位單位（名為 CA2）取得要 www.contoso.com 的憑證。 然後，裝載虛設 www.contoso.com 網站的實體可能會損毀用戶端或伺服器的 DNS 快取，以將 www.contoto.com 指向其假網站。 終端使用者將會看到來自 CA2 的憑證，而且可以直接確認並聯機到假的網站。 使用來時，用戶端會對 DNS 伺服器提出要求，以供 contoso.com 要求 TLSA 記錄，並瞭解 www.contoso.com 的憑證是 CA1 的問題。 如果有來自另一個 CA 的憑證，則連接會中止。

## <a name="unknown-record-support"></a>未知的記錄支援

「未知的記錄」是指 DNS 伺服器不知道其 .RDATA 格式的 RR。 新增對未知記錄 (RFC 3597) 類型的支援，表示您可以將不支援的記錄類型新增至二進位線上格式的 Windows DNS 伺服器區域。 Windows caching 解析程式已經有處理未知記錄類型的能力。 Windows DNS 伺服器不會對未知記錄進行任何記錄特定的處理，但會在收到查詢時傳迴響應。

## <a name="ipv6-root-hints"></a>IPv6 根目錄提示

IANA 所發佈的 IPV6 根提示已新增至 windows DNS 伺服器。 網際網路名稱查詢現在可以使用 IPv6 根功能變數名稱伺服器來執行名稱解析。

## <a name="windows-powershell-support"></a>Windows PowerShell 支援

以下是 Windows Server 2016 中引進的新 Windows PowerShell Cmdlet 和參數。

-   **新增-DnsServerRecursionScope**。 此 Cmdlet 會在 DNS 伺服器上建立新的遞迴範圍。 DNS 原則會使用遞迴範圍來指定 DNS 查詢中所使用的轉寄站清單。

-   **移除-DnsServerRecursionScope**。 此 Cmdlet 會移除現有的遞迴範圍。

-   **設定-DnsServerRecursionScope**。 此 Cmdlet 會變更現有遞迴範圍的設定。

-   **DnsServerRecursionScope**。 此 Cmdlet 會抓取現有遞迴範圍的相關資訊。

-   **新增-DnsServerClientSubnet**。 此 Cmdlet 會建立新的 DNS 用戶端子網。 DNS 原則會使用子網來識別 DNS 用戶端所在的位置。

-   **移除-DnsServerClientSubnet**。 此 Cmdlet 會移除現有的 DNS 用戶端子網。

-   **設定-DnsServerClientSubnet**。 此 Cmdlet 會變更現有 DNS 用戶端子網的設定。

-   **DnsServerClientSubnet**。 此 Cmdlet 會抓取現有 DNS 用戶端子網的相關資訊。

-   **新增-DnsServerQueryResolutionPolicy**。 此 Cmdlet 會建立新的 DNS 查詢解析原則。 DNS 查詢解析原則可用來根據不同的準則來指定查詢的回應方式。

-   **移除-DnsServerQueryResolutionPolicy**。 此 Cmdlet 會移除現有的 DNS 原則。

-   **設定-DnsServerQueryResolutionPolicy**。 此 Cmdlet 會變更現有 DNS 原則的設定。

-   **DnsServerQueryResolutionPolicy**。 此 Cmdlet 會抓取現有 DNS 原則的相關資訊。

-   **啟用-DnsServerPolicy**。 此 Cmdlet 會啟用現有的 DNS 原則。

-   **停用-DnsServerPolicy**。 此 Cmdlet 會停用現有的 DNS 原則。

-   **新增-DnsServerZoneTransferPolicy**。 此 Cmdlet 會建立新的 DNS 伺服器區域傳輸原則。 DNS 區域轉移原則會指定是否要根據不同的準則來拒絕或略過區域轉送。

-   **移除-DnsServerZoneTransferPolicy**。 此 Cmdlet 會移除現有的 DNS 伺服器區域傳輸原則。

-   **設定-DnsServerZoneTransferPolicy**。 此 Cmdlet 會變更現有 DNS 伺服器區域傳輸原則的設定。

-   **DnsServerResponseRateLimiting**。 此 Cmdlet 會取出 RRL 設定。

-   **設定-DnsServerResponseRateLimiting**。 此 Cmdlet 會變更 RRL 設定也。

-   **新增-DnsServerResponseRateLimitingExceptionlist**。 此 Cmdlet 會在 DNS 伺服器上建立一個 RRL 例外狀況清單。

-   **DnsServerResponseRateLimitingExceptionlist**。 此 Cmdlet 會捕獲 RRL excception 清單。

-   **移除-DnsServerResponseRateLimitingExceptionlist**。 此 Cmdlet 會移除現有的 RRL 例外清單。

-   **設定-DnsServerResponseRateLimitingExceptionlist**。 此 Cmdlet 會變更 RRL 例外狀況清單。

-   **新增-DnsServerResourceRecord**。 此 Cmdlet 已更新為支援未知的記錄類型。

-   **DnsServerResourceRecord**。 此 Cmdlet 已更新為支援未知的記錄類型。

-   **移除-DnsServerResourceRecord**。 此 Cmdlet 已更新為支援未知的記錄類型。

-   **設定-DnsServerResourceRecord**。 此 Cmdlet 已更新為支援未知的記錄類型

如需詳細資訊，請參閱下列 Windows Server 2016 Windows PowerShell 命令參考主題。

- [DnsServer 模組](/powershell/module/dnsserver/)
- [Set-dnsclient 模組](/powershell/module/dnsclient/)

## <a name="additional-references"></a>其他參考資料

-   [DNS 用戶端的新功能](What-s-New-in-DNS-Client.md)

---
title: 在 Windows Server 的 DNS 伺服器中的新功能
description: 本主題提供概觀 DNS 伺服器，在 Windows Server 2016 和較新版本中的新功能
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3ddb8920c045f231dcf5286283d9895ef6ffff47
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>在 Windows Server 的 DNS 伺服器中的新功能

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題描述新增或變更 Windows Server 2016 中的網域名稱系統 」 (DNS) 伺服器功能。  
  
Windows Server 2016 DNS 伺服器提供下列幾個方面美化的支援。  
  
|功能|新的或改進|描述|  
|-----------------|-------------------|---------------|  
|DNS 原則|新|您可以設定 DNS 原則，若要指定 DNS 伺服器回應 DNS 查詢的方式。 DNS 回應可以根據 client IP 位址 （位置），以及幾個其他的參數時間。 定位感知 DNS、 流量管理、 負載平衡、 split-brain DNS 及其他案例，可讓 DNS 原則。|  
|回應速率限制 (RRL)|新|您可以讓您的 DNS 伺服器上的回應速率限制。 執行此動作，您避免使用您的 DNS 伺服器起始阻斷服務 DNS client 攻擊惡意系統。|  
|DNS 為基礎的驗證的命名實體 (DANE)|新|您可以使用 TLSA （傳輸層級的安全性驗證） 記錄 DNS 用狀態哪些 CA 應該針對您的網域名稱的憑證，以提供的資訊。 如此可防止位置某人可能會損壞 DNS 快取指向他們自己的網站，並提供他們所發行的其他 CA 憑證在中央男人攻擊。|  
|無法辨識的記錄支援|新|您可以新增記錄明確不支援的 Windows DNS 伺服器使用未知的記錄功能。|  
|IPv6 根提示|新|您可以使用的原生 IPV6 根提示支援執行網際網路名稱解析使用 IPV6 根伺服器。|  
|Windows PowerShell 支援|已改進|新的 Windows PowerShell cmdlet 可用的 DNS 伺服器。|  
  
## <a name="dns-policies"></a>DNS 原則

您可以使用地理位置資料傳輸管理依據時間、 智慧型 DNS 回應 DNS 原則管理設定為 split\ 蛋部署，單一 DNS 伺服器上 DNS 查詢套用篩選。 下列項目提供更多有關這些功能的詳細資料。

-   **應用程式負載平衡。** 當您完成部署多次應用程式的不同位置時，您可以使用 DNS 原則，以平衡流量載入之間動態應用程式的配置流量載入的不同的應用程式執行個體。

-   **Geo\ 位置型流量管理。** 您可以使用 DNS 原則，以允許回應 DNS client 查詢 client 和資源嘗試 client 連接的地理位置為基礎的主要和次要 DNS 伺服器 client 提供接近資源的 IP 位址。 

-   **請分割大腦 DNS。** 、 Split\ 蛋 DNS 記錄分為不同區域範圍上相同的 DNS 伺服器，並 DNS 用收到依據戶端是否內部或外部用的回應。 您可以設定 split\ 蛋 DNS 整合 Active Directory 區域或區域獨立 DNS 伺服器上。

-   **篩選。** 您可以設定來建立查詢篩選器根據您所提供的準則 DNS 原則。 查詢篩選 DNS 原則中的，讓您設定根據 DNS 查詢和傳送 DNS 查詢 DNS client 的方式自訂回應 DNS 伺服器。 
-   **Forensics。** 您可以將 non\ 存在 IP 位址，而不將它們想瀏覽電腦惡意 DNS 用使用 DNS 原則。

-   **一天的時間型重新導向。** 您可以將應用程式流量跨不同的分散執行個體的應用程式，使用 DNS 原則根據一天的時間，使用 DNS 原則。 
  
您也可以使用 DNS 原則，針對 Active Directory 整合 DNS 區域。

如需詳細資訊，請查看[DNS 原則案例指南](deploy/DNS-Policies-Overview.md)。

## <a name="response-rate-limiting"></a>回應速率限制

您可以設定 RRL 控制要如何應付 DNS client 要求時您的伺服器收到目標相同 client 的數個要求。 執行此動作，您可以防止其他人使用您的 DNS 伺服器阻服務 (Dos) 攻擊給我們。 例如，自動機器人程式網路要求傳送到您為名字使用電腦的 IP 位址的 DNS 伺服器。 RRL，而您的 DNS 伺服器可能會回應所有的要求，流第三個電腦。 當您使用 RRL 時，您可以設定的下列設定：  
  
-   **回應秒**。 這是最大的次數到 client 一秒中提供相同的回應。  
  
-   **錯誤秒**。 這是最大的次數用中一秒傳送錯誤回應。  
  
-   **視窗**。 這是數秒鐘的回應 client 將會暫時停用如果太多要求。  
  
-   **遺漏速率**。 這是 DNS 伺服器會暫停回應期間查詢回應的頻率。 例如，伺服器暫停回應 client 到 10 秒，且遺漏速率 5，如果伺服器會仍然回應一個查詢傳送每個 5 查詢。 這可讓合法戶端取得 DNS 伺服器會套用的回應速度子網路上 FQDN 限制時，即使的回應。  
  
-   **TC 速率**。 這用來告知您可以嘗試時回應 client 會暫停的 TCP 連接 client。 例如，如果 TC 速率是 3 伺服器暫停指定 client 的回應，伺服器會發出要求收到每 3 個查詢的 TCP 連接。 請確定您的值為 TC 速率低於遺漏速度，讓 client 透過 TCP 連接之前遺漏回應的選項。  
  
-   **最大的回應**。 這是最大回應伺服器會發給 client 時回應會暫停的數目。  
  
-   **白色清單網域**。 這是要排除 RRL 設定的網域清單。  
  
-   **白色清單子網路**。 這是要排除 RRL 設定子網路清單。  
  
-   **白色清單伺服器介面**。 這是要排除 RRL 設定的 DNS 伺服器介面的清單。  
  
## <a name="dane-support"></a>DANE 支援

您可以使用 DANE 支援 \ （RFC 6394 和 6698\） 來指定您 DNS 用來 CA 應該憑證所發行的網域名稱裝載在您的 DNS 伺服器。 如此可防止其他人就能破壞 DNS 快取，並其本身的 IP 位址指向 DNS 名稱中中央男人攻擊一種。  
  
例如，想像主機安全的網站時，在 www.contoso.com 使用 SSL 使用名 CA1 已知授權的憑證。 其他人仍可能的不同，不讓-知，憑證授權單位名 CA2 取得 www.contoso.com 的憑證。 然後，裝載假 www.contoso.com 網站的實體可能無法損壞 DNS 快取 client 或伺服器 www.contoto.com 指向他們假的網站。 使用者從 CA2，會顯示憑證可能只是確認它並連接到假的網站。 DANE，client 想讓要求 contoso.com TLSA 記錄要求的 DNS 伺服器與了 www.contoso.com 憑證已 CA1 的問題。 如果出現從另一部 CA 憑證，則會中止連接。  
  
## <a name="unknown-record-support"></a>無法辨識的記錄支援

「 未知記錄 」 是 RR RDATA 格式不已知 DNS 伺服器。 新增的支援未知的記錄 (RFC 3597) 類型表示，您可以將不支援使用碼表進行類型新增至 Windows DNS 伺服器區域網路上二進位格式。 Windows 快取解析已經處理不明的記錄類型的能力。 Windows DNS 伺服器會執行任何使用碼表進行特定的處理不明記錄，但將會傳送入回應如果查詢接收到它。  
  
## <a name="ipv6-root-hints"></a>IPv6 根提示

IPV6 根提示，IANA、 由已新增至 windows DNS 伺服器。 網際網路名稱查詢現在可以使用 IPv6 根伺服器執行名稱解析度。

## <a name="windows-powershell-support"></a>Windows PowerShell 支援

下列新的 Windows PowerShell cmdlet 和參數引進 Windows Server 2016。
  
-   **新增 DnsServerRecursionScope**。 這個 cmdlet 建立新的領域遞迴 DNS 伺服器上。 遞迴範圍 DNS 原則用於指定轉寄器會使用 DNS 查詢中的清單。  
  
-   **移除-DnsServerRecursionScope**。 這個 cmdlet 移除現有的遞迴範圍。  
  
-   **設定-DnsServerRecursionScope**。 這個 cmdlet 變更現有遞迴領域的設定。  
  
-   **取得-DnsServerRecursionScope**。 這個 cmdlet 擷取現有遞迴領域有關的資訊。  
  
-   **新增 DnsServerClientSubnet**。 這個 cmdlet 會建立新的 DNS client 子網路。 子網路會使用 DNS 原則找出 DNS client 所在的位置。  
  
-   **移除-DnsServerClientSubnet**。 這個 cmdlet 移除現有的 DNS client 子網路。  
  
-   **設定-DnsServerClientSubnet**。 這個 cmdlet 變更現有的 DNS client 子網路的設定。  
  
-   **取得-DnsServerClientSubnet**。 這個 cmdlet 擷取現有 DNS client 子網路相關資訊。  
  
-   **新增 DnsServerQueryResolutionPolicy**。 這個 cmdlet 建立新的 DNS 查詢解析度原則。 用於指定 DNS 查詢解析度原則為何，或如果查詢為回應，以不同的條件。  
  
-   **移除-DnsServerQueryResolutionPolicy**。 這個 cmdlet 移除現有的 DNS 原則。  
  
-   **設定-DnsServerQueryResolutionPolicy**。 這個 cmdlet 變更現有的 DNS 原則的設定。  
  
-   **取得-DnsServerQueryResolutionPolicy**。 這個 cmdlet 擷取現有 DNS 原則的相關資訊。  
  
-   **讓-DnsServerPolicy**。 這個 cmdlet 可讓您現有的 DNS 原則。  
  
-   **停用-DnsServerPolicy**。 這個 cmdlet 停用現有 DNS 原則。  
  
-   **新增 DnsServerZoneTransferPolicy**。 這個 cmdlet 會建立新的 DNS 伺服器區傳輸原則。 DNS 區域傳輸原則指定是否拒絕或忽略區域轉送根據不同的條件。  
  
-   **移除-DnsServerZoneTransferPolicy**。 這個 cmdlet 移除現有的 DNS 伺服器區傳輸原則。  
  
-   **設定-DnsServerZoneTransferPolicy**。 這個 cmdlet 變更現有的 DNS 伺服器區傳輸原則設定。  
  
-   **取得-DnsServerResponseRateLimiting**。 這個 cmdlet 擷取 RRL 設定。  
  
-   **設定-DnsServerResponseRateLimiting**。 這個 cmdlet 變更 RRL settigns。  
  
-   **新增 DnsServerResponseRateLimitingExceptionlist**。 這個 cmdlet DNS 伺服器上建立 RRL 例外清單。  
  
-   **取得-DnsServerResponseRateLimitingExceptionlist**。 這個 cmdlet 擷取 RRL excception 清單。  
  
-   **移除-DnsServerResponseRateLimitingExceptionlist**。 這個 cmdlet 現有 RRL 例外清單中移除。  
  
-   **設定-DnsServerResponseRateLimitingExceptionlist**。 這個 cmdlet 變更 RRL 例外清單。  
  
-   **新增 DnsServerResourceRecord**。 這個 cmdlet 已更新以支援未知的記錄類型。  
  
-   **取得-DnsServerResourceRecord**。 這個 cmdlet 已更新以支援未知的記錄類型。  
  
-   **移除-DnsServerResourceRecord**。 這個 cmdlet 已更新以支援未知的記錄類型。  
  
-   **設定-DnsServerResourceRecord**。 這個 cmdlet 已更新以支援未知的記錄類型

如需詳細資訊，下列 Windows Server 2016 Windows PowerShell 命令參考主題。

- [DnsServer 模組](https://technet.microsoft.com/itpro/powershell/windows/dns-server/index)
- [DnsClient 模組](https://technet.microsoft.com/itpro/powershell/windows/dns-client/index)

## <a name="see-also"></a>也了  
  
-   [DNS Client 中的新功能](What-s-New-in-DNS-Client.md)  
  

  


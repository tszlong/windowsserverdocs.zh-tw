---
title: "TLS-SSL (Schannel SSP) 概觀"
description: "Windows Server 安全性"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c8836345-16bb-4dcc-8d2b-2b9b687456a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: b846ed54a1f7c8ef7a85ea9f836ffa75d0036ae6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="overview-of-tls---ssl-schannel-ssp"></a>TLS-SSL (Schannel SSP) 簡介

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題適用於 IT 專業人員描述的功能在 Schannel 安全性支援提供者 (SSP)，包括傳輸層級的安全性 (TLS)、 安全通訊端層 (SSL)，以及資料流傳輸層級的安全性 (DTLS) 驗證通訊協定，適用於 Windows Server 2012 R2、 Windows Server 2012、 Windows 8.1 和 Windows 8 的變更。

Schannel 是安全性支援提供者 (SSP) 實作 SSL、 TLS 和 DTLS 網際網路標準驗證通訊協定。 安全性支援提供者介面 (SSPI) 是使用 windows 來執行的安全性相關的功能包括驗證 API。 常見介面數個安全性支援提供者 （層），包括 Schannel SSP.SSPI 功能

如需 Microsoft 實作 TLS 和 SSL Schannel SSP 中的，請查看[TLS 日 SSL 技術參考 (2003)](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)。


##<a name="tlsssl-schannel-ssp-features"></a>TLS SSL (Schannel SSP) 功能
下列描述 TLS Schannel SSP.中的功能

### <a name="tls-session-resumption"></a>TLS 工作階段回復
傳輸層級的安全性 (TLS) 通訊協定，元件 Schannel 安全性支援提供者，用來傳送未受信任的網路上的應用程式間的資料的安全。 可以使用 TLS 日 SSL 驗證伺服器和 client 電腦，以及加密已驗證的對象之間的訊息。

經常 TLS 連接到伺服器的裝置需要重新連接，由於工作階段到期。 Windows 8.1 和 Windows Server 2012 R2 現在支援 RFC 5077 （而不需要伺服器端狀態 TLS 工作階段回復）。 Windows Phone 和 Windows RT 裝置提供此修改：

-   在伺服器上資源減少的使用量

-   減少的頻寬，可改善 client 連接的效率

-   減少的因為 resumptions 連接的 TLS 交換所花費的時間。

> [!NOTE]
> Windows 8 中加入的 RFC 5077 client 端實作。

無狀態 TLS 工作階段回復的相關資訊，會看到 IETF 文件[RFC 5077。](http://www.ietf.org/rfc/rfc5077)

### <a name="application-protocol-negotiation"></a>應用程式通訊協定交涉
 Windows Server 2012 R2 和 Windows 8.1 支援 client 端 TLS 應用程式通訊協定交涉讓應用程式可如何利用通訊協定 HTTP 2.0 標準開發的一部分，使用者可以存取 online services Google 和 Twitter 使用應用程式執行 SPDY 通訊協定。

**它的運作方式**

Client 和伺服器應用程式可讓應用程式通訊協定交涉擴充功能來提供支援的應用程式通訊協定 Id 中的順序喜好設定的清單。 TLS client 表示它支援的應用程式通訊協定交涉，包括 ClientHello 訊息中的一份 client 支援的通訊協定的應用程式層通訊協定交涉 (ALPN) 擴充功能。

當 TLS client 伺服器提出要求時，TLS 伺服器讀取最慣用的應用程式通訊協定 client 也支援，其支援的通訊協定清單。 如果找到通訊協定，選取通訊協定 id 看伺服器，並像往常一樣繼續交換。 如果有任何通用應用程式通訊協定，伺服器傳送嚴重交換失敗警示。

### <a name="BKMK_TrustedIssuers"></a>信任的發行者 client 驗證的管理
需要使用 SSL 或 TLS client 電腦的驗證時，可以設定伺服器來傳送的發行者受信任的憑證清單。 這份清單包含伺服器將信任的憑證發行者的設定，如果選取的 client 憑證至於有多個憑證 client 電腦的提示。 此外，針對設定信任的發行者清單必須進行驗證 client 電腦傳送到伺服器的憑證鏈結。

之前，Windows Server 2012 和 Windows 8，應用程式或處理程序使用 Schannel SSP （包括 HTTP.sys 和 IIS） 可能會提供其支援 Client 驗證透過憑證信任清單 (CTL) 信任的發行者的清單。

在 Windows Server 2012 和 Windows 8，變更驗證程序，讓：

-   已不再支援 CTL 型受信任的發行者清單管理。

-   行為傳送信任的發行者清單預設為關閉： SendTrustedIssuerList 登錄金鑰的預設值是現在 0 （預設為關閉） 而不是 1。

-   保留到先前的 Windows 作業系統版本的相容性。

**這會新增值為何？**

開始使用 Windows Server 2012，已使用的憑證存放區根據實作取代 CTL 使用。 這可透過 PowerShell 提供者，以及命令列工具，例如 certutil.exe 現有的憑證管理 commandlets 更熟悉性。

雖然 Schannel SSP 支援 (16 KB) 受信任的憑證授權單位清單的最大大小保持不變與 Windows Server 2008 R2、 Windows Server 2012 有中新的專用之憑證存放區 client 驗證的發行者，該訊息中不包含不相關的憑證。

**它如何運作？**

在 Windows Server 2012，信任的發行者清單是使用設定憑證存放區。有一個預設全球電腦憑證存放區，另一個是選擇性的每個網站。 將會判斷清單的來源，如下所示：

-   如果有特定的憑證存放區的網站設定，將會做為來源

-   如果不到憑證應用程式定義的市集中，就會檢查 Schannel **Client 驗證的發行者**儲存在本機電腦上，若憑證已，會使用該市集做為來源。 如果不找到任何憑證任一市集中，就會檢查受信任的根網上商店。

-   如果這兩個全球或本機存放區包含憑證，Schannel 提供者將會使用**信任的根 Certifictation 授權**儲存為 「 信任的發行者清單的來源。 （這是針對 Windows Server 2008 R2 的行為）。

如果**信任的根 Certifictation 授權**使用網上商店包含 （自我簽署） 根憑證授權單位發行者憑證混合、 發行者 CA 憑證將會預設傳送到伺服器。

**如何設定要使用的憑證存放區的受信任的發行者 Schannel**

Windows Server 2012 中的 Schannel SSP 架構將會預設使用如上文所述的商店管理受信任的發行者清單。 您仍然可以使用現有的憑證管理 commandlets PowerShell 提供者，以及命令列工具，例如 Certutil 管理的憑證。

用於管理憑證使用提供者 PowerShell 相關資訊，請查看[在 Windows 中 AD CS 管理 Cmdlet](https://technet.microsoft.com/library/hh848365(v=wps.620).aspx)。

用於管理憑證使用憑證的公用程式的相關資訊，請查看[certutil.exe](https://technet.microsoft.com/library/cc732443.aspx)。

哪些資料，包括應用程式定義市集中，定義 Schannel 認證的詳細資訊，請查看[SCHANNEL_CRED 結構 (Windows)](https://msdn.microsoft.com/library/windows/desktop/aa379810(v=vs.85).aspx)。

**預設值的信任模式**

有三種 Client 驗證信任模式支援 Schannel 提供者。 信任模式可讓您控制如何執行 client 的憑證鏈結的驗證並由呼叫完成」ClientAuthTrustMode」在 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel 控制全系統設定.

|值。|信任模式|描述|
|-----|-------|--------|
|0|電腦信任 （預設值）|需要 client 憑證的受信任的發行者清單中的憑證來發行。|
|1|專屬根信任|需要 client 憑證到呼叫特定受信任的發行者網上商店中所包含的根憑證鏈結。 信任的發行者清單中的發行者也必須發出憑證|
|2|專屬 CA 信任|需要 client 中繼 CA 憑證或根憑證呼叫特定的憑證鏈結信任的發行者網上商店。|

因為信任的發行者設定問題驗證失敗的詳細資訊，會看到知識庫文件[280256](https://support.microsoft.com/kb/2802568)。

### <a name="BKMK_SNI"></a>TLS 伺服器名稱指示器 (SNI) 擴充功能的支援
伺服器名稱指示功能延伸 SSL 和 TLS 通訊協定，允許伺服器的適當驗證時單一伺服器上執行許多虛擬映像。 若要適當保護 client 的電腦 」 和 「 伺服器之間的通訊，client 電腦會要求數位憑證從伺服器。 回應要求伺服器，並傳送的憑證之後，client 的電腦檢查、 使用它來加密通訊，並繼續以一般的要求回應換貨。 不過，virtual 裝載案例中，在多個網域，每一個都有它自己可能不同的憑證，裝載一部伺服器上。 在這種情形下，伺服器必須事先得知 client 的電腦所傳送的憑證。 SNI 允許 client 電腦通知目標網域稍早的通訊協定，和這可以讓伺服器正確選取適當的憑證。

**這會新增值為何？**

此額外的功能：

-   可讓您在主機上的單一 IP 和連接埠組合多 SSL 網站

-   在單一網頁伺服器裝載多 SSL 網站時，可減少記憶體使用量

-   可讓使用者更多同時連接至我 SSL 網站

-   讓您可以透過電腦介面使用者提供提示 client 驗證程序期間選取正確的憑證。

**它的運作方式**

Schannel SSP 維護記憶體中的快取的戶端允許 client 連接狀態。 這可以讓 client 電腦後續造訪快速連接到不完整的 SSL 交換受 SSL 伺服器。  此有效的憑證管理使用允許更多裝載單一的 Windows Server 2012，相較於舊版作業系統的網站。

使用者憑證選取已改良，讓您建立的使用者提供提示選擇哪一個可能的憑證發行者名稱的清單。 這份清單，並可以使用群組原則設定。

### <a name="BKMK_DTLS"></a>資料流 Tls (DTLS)
已新增 DTLS 1.0 通訊協定 Schannel 安全性支援提供者。 DTLS 通訊協定提供通訊隱私權，資料流通訊協定。 通訊協定允許 client 日伺服器應用程式的方式是設計用來防止竊取、 竄改，或冒名訊息通訊。 DTLS 通訊協定根據傳輸層級的安全性 (TLS) 通訊協定，並提供相同的安全保證減少使用 IPsec 或設計的應用程式自訂層安全性通訊協定。

**這會新增值為何？**

資料包都有的串流媒體，例如遊戲或受保護的影片會議。 新增 DTLS 通訊協定 Schannel 提供者可讓您使用熟悉的 Windows SSPI 模型設定安全性 client 電腦之間的伺服器通訊。 DTLS 故意被設計為類似 TLS 以最新的安全性發明最小化並發揮最大的驗證碼與基礎結構重複使用。

**它的運作方式**

使用 DTLS UDP 上的應用程式可以使用在 Windows Server 2012 和 Windows 8 中的 SSPI 模型。 某些密碼套件可供設定，您可以設定 TLS 類似。 Schannel 會繼續使用利用 FIPS 140 認證，在 Windows Vista 的 CNG 密碼編譯提供者。

### <a name="BKMK_Deprecated"></a>過時的功能
在適用於 Windows Server 2012 和 Windows 8 Schannel SSP，有任何已取代的功能。

## <a name="see-also"></a>也了
-   [私人雲端安全性模型包裝功能](https://social.technet.microsoft.com/wiki/contents/articles/6756.private-cloud-security-model-wrapper-functionality.aspx)




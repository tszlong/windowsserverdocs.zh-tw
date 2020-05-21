---
title: TLS-SSL （安全通道 SSP）總覽
description: Windows Server 安全性
ms.prod: windows-server
ms.technology: security-tls-ssl
ms.topic: article
ms.assetid: c8836345-16bb-4dcc-8d2b-2b9b687456a3
author: justinha
ms.author: justinha
manager: brianlic
ms.date: 05/16/2018
ms.openlocfilehash: 105225736d6b883e8451aa599af1937068ebe43d
ms.sourcegitcommit: f22e4d67dd2a153816acf8355e50319dbffc5acf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/18/2020
ms.locfileid: "83546558"
---
# <a name="overview-of-tls---ssl-schannel-ssp"></a>TLS-SSL （安全通道 SSP）的總覽

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows 10

本主題適用于 IT 專業人員，說明安全通道安全性支援提供者（SSP）中的功能變更，其中包括傳輸層安全性（TLS）、安全通訊端層（SSL）和資料包傳輸層安全性（DTLS）驗證通訊協定，適用于 Windows Server 2012 R2、Windows Server 2012、Windows 8.1 和 Windows 8。

安全通道是安全性支援提供者 (SSP)，可以實作 SSL、TLS 及 DTLS 網際網路標準驗證通訊協定。 安全性支援提供者介面 (SSPI) 是 Windows 系統所使用的 API，可以執行包含驗證在內的安全性相關功能。 SSPI 可以做為包含安全通道 SSP 在內的數個安全性支援提供者 (SSP) 的常用介面來運作。

如需 Microsoft 在安全通道 SSP 中執行 TLS 和 SSL 的詳細資訊，請參閱[TLS/SSL 技術參考（2003）](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)。


## <a name="tlsssl-schannel-ssp-features"></a>TLS/SSL （安全通道 SSP）功能
以下說明 Schannel SSP 中的 TLS 功能。

### <a name="tls-session-resumption"></a>TLS 工作階段繼續
傳輸層安全性 (TLS) 通訊協定是安全通道安全性支援提供者元件，可用來保護跨不受信任網路的應用程式之間所傳送的資料。 TLS/SSL 可用來驗證伺服器與用戶端電腦，也可用來加密已驗證對象之間的訊息。

將 TLS 連接到伺服器的裝置經常因工作階段逾時而需要重新連線。 Windows 8.1 和 Windows Server 2012 R2 現在支援 RFC 5077 （不含伺服器端狀態的 TLS 會話繼續）。 此修改為 Windows Phone 和 Windows RT 裝置提供：

-   減少伺服器上的資源使用量

-   降低頻寬，改善用戶端連線的效率

-   減少因繼續連線的 TLS 信號交換所花費的時間。

> [!NOTE]
> RFC 5077 的用戶端實作已加入 Windows 8 中。

如需無狀態 TLS 會話繼續的詳細資訊，請參閱 IETF 檔[RFC 5077。](http://www.ietf.org/rfc/rfc5077)

### <a name="application-protocol-negotiation"></a>應用程式通訊協定交涉
 Windows Server 2012 R2 和 Windows 8.1 支援用戶端 TLS 應用程式協定協商，因此應用程式可以利用通訊協定作為 HTTP 2.0 標準開發的一部分，而使用者可以使用執行 SPDY 通訊協定的應用程式存取線上服務（例如 Google 和 Twitter）。

**運作方式**

用戶端和伺服器應用程式可提供支援的應用程式通訊協定識別碼來啟用應用程式通訊協定交涉延伸模組 (依喜好設定的遞減順序)。 TLS 用戶端可指示它支援應用程式通訊協定交涉，方法是在 ClientHello 訊息中，加入包含用戶端支援的通訊協定清單的應用程式層通訊協定交涉 (ALPN) 延伸模組。

當 TLS 用戶端對伺服器提出要求時，TLS 伺服器會針對最慣用的應用程式通訊協定 (用戶端也支援) 讀取其支援的通訊協定清單。 如果發現這類通訊協定，伺服器會回應所選的通訊協定識別碼，並如往常一樣繼續信號交換。 如果沒有一般的應用程式通訊協定，伺服器會傳送信號交換嚴重失敗警示。

### <a name="management-of-trusted-issuers-for-client-authentication"></a><a name="BKMK_TrustedIssuers"></a>用戶端驗證受信任簽發者的管理
當用戶端電腦的驗證需要使用 SSL 或 TLS 時，您可以設定伺服器傳送受信任的憑證簽發者清單。 這份清單包含伺服器將會信任的一組憑證簽發者，並為用戶端電腦提供出現多個憑證時，應選擇使用之用戶端憑證的提示。 此外，用戶端電腦傳送到伺服器的憑證鏈結必須向設定的信任簽發者清單進行驗證。

在 Windows Server 2012 和 Windows 8 之前，使用安全通道 SSP （包括 HTTP.SYS 和 IIS）的應用程式或進程，可以透過憑證信任清單（CTL）提供支援用戶端驗證的受信任簽發者清單。

在 Windows Server 2012 和 Windows 8 中，對基礎驗證程式進行了變更，讓：

-   不再支援 CTL 式受信任簽發者清單的管理。

-   預設會關閉傳送受信任簽發者清單的行為： SendTrustedIssuerList 登錄機碼的預設值現在是0（預設為關閉），而不是1。

-   保留與舊版 Windows 作業系統的相容性。

> [!NOTE]
> 如果用戶端應用程式已啟用系統對應程式，而且您已設定，則系統對應工具 `SendTrustedIssuers` 會將新增至簽發者 `CN=NT Authority` 清單。


**這會新增什麼值？**

從 Windows Server 2012 開始，使用 CTL 已取代為以憑證存放區為基礎的執行。 透過 PowerShell 提供者的現有憑證管理 Commandlet 和命令列工具 (如 certutil.exe)，提供您更熟悉的管理方式。

雖然安全通道 SSP 支援的受信任憑證授權單位單位清單大小上限（16 KB）與 Windows Server 2008 R2 中的相同，但在 Windows Server 2012 中，用戶端驗證簽發者會有新的專用憑證存放區，因此訊息中不會包含不相關的憑證。

**運作方式**

在 Windows Server 2012 中，會使用憑證存放區來設定信任的簽發者清單;一個預設的全域電腦憑證存放區，另一個是每個網站都是選擇性的。 清單的來源將取決於下列條件：

-   如果已為網站設定特定的認證存放區，則會將之做為來源

-   如果應用程式定義的存放區中沒有任何憑證，則安全通道會檢查本機電腦上的 [用戶端驗證簽發者]**** 存放區，且如果出現憑證，則將該存放區做為來源。 如果任一存放區都找不到任何憑證，則會檢查信任根目錄存放區。

-   如果全域或本機存放區都沒有包含憑證，Schannel 提供者將會使用**受信任的根憑證授權**單位存放區做為受信任簽發者清單的來源。 （這是 Windows Server 2008 R2 的行為。）

如果使用的**受根信任證書授權**單位存放區包含根（自我簽署）和憑證授權單位單位（CA）簽發者憑證，則依預設只會將 CA 簽發者憑證傳送至伺服器。

**如何設定 Schannel 以使用受信任簽發者的憑證存放區**

Windows Server 2012 中的安全通道 SSP 架構預設會使用上述的存放區來管理受信任的簽發者清單。 您仍然可以使用 PowerShell 提供者現有的憑證管理 Commandlet 和命令列工具 (例如 Certutil) 來管理憑證。

如需使用 PowerShell 提供者來管理憑證的相關資訊，請參閱 [Windows 中的 AD CS 管理 Cmdlet](https://technet.microsoft.com/library/hh848365(v=wps.620).aspx)。

如需使用憑證公用程式來管理憑證的相關資訊，請參閱 [certutil.exe](https://technet.microsoft.com/library/cc732443.aspx)。

如需針對安全通道認證定義的資料內容相關資訊 (包括應用程式定義的存放區)，請參閱 [SCHANNEL_CRED 結構 (Windows)](https://msdn.microsoft.com/library/windows/desktop/aa379810(v=vs.85).aspx)。

**信任模式的預設值**

安全通道提供者支援三種用戶端驗證信任模式。 [信任] 模式會控制用戶端憑證鏈的驗證如何執行，而且是由 HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel. 之下的 REG_DWORD "ClientAuthTrustMode" 所控制的全系統設定。

|值|信任模式|描述|
|-----|-------|--------|
|0|機器信任 (預設)|要求用戶端憑證必須由受信任簽發者清單中的憑證所簽發。|
|1|獨佔根信任|要求用戶端憑證要鏈結至包含在呼叫者指定之受信任簽發者存放區中的根憑證。 憑證還必須由受信任簽發者清單中的簽發者簽發。|
|2|獨佔 CA 信任|要求用戶端憑證要鏈結至中繼 CA 憑證，或鏈結至呼叫者指定之受信任簽發者存放區中的根憑證。|

如需因受信任簽發者設定問題所發生的驗證失敗詳細資訊，請參閱 Microsoft 知識庫文章 [280256](https://support.microsoft.com/kb/2802568)。

### <a name="tls-support-for-server-name-indicator-sni-extensions"></a><a name="BKMK_SNI"></a>伺服器名稱指標（SNI）延伸的 TLS 支援
伺服器名稱指示器功能會延伸 SSL 和 TLS 通訊協定，當單一伺服器上有許多虛擬映像正在執行時，允許對伺服器進行適當的識別。 為了適當保護用戶端電腦與伺服器之間的通訊安全，用戶端電腦會要求伺服器提供數位憑證。 在伺服器回應要求並傳送憑證之後，用戶端電腦會檢驗該憑證、使用它來加密通訊，並繼續進行正常的要求回應交換。 不過，在虛擬主機案例中，會在一部伺服器上裝載多個網域，每個網域都各自擁有可能完全不同的憑證。 在這種情況下，伺服器沒有方法可以預先得知要將哪個憑證傳送到用戶端電腦。 SNI 允許用戶端電腦在通訊協定中提前通知目標網域，而這讓伺服器能夠正確地選取適當的憑證。

**這會新增什麼值？**

這個額外功能：

-   讓您能夠在單一 IP 和連接埠組合中裝載多個 SSL 網站

-   在單一網頁伺服器上裝載多個 SSL 網站時，可以降低記憶體使用率

-   允許更多使用者同時連線到我的 SSL 網站

-   允許您透過電腦介面為使用者提供提示，以便在用戶端驗證程序期間選取正確的憑證。

**運作方式**

安全通道 SSP 會維護針對用戶端所允許的用戶端連線狀態的記憶體內部快取。 這可讓用戶端電腦快速重新連線到 SSL 伺服器，而不需在後續造訪時受限於完整的 SSL 交握。  相較于舊版作業系統版本，此有效使用憑證管理可讓更多網站裝載在單一 Windows Server 2012 上。

藉由讓您建構可能的憑證簽發者名稱清單 (這可以提示使用者要選擇哪一個)，強迫使用者選取憑證。 這個清單可以使用群組原則來設定。

### <a name="datagram-transport-layer-security-dtls"></a><a name="BKMK_DTLS"></a>資料包傳輸層安全性 (DTLS)
DTLS 版本 1.0 通訊協定已新增到安全通道安全性支援提供者。 DTLS 通訊協定可以為資料包通訊協定提供通訊私密性。 這個通訊協定讓用戶端/伺服器應用程式能夠以設計來防止竊聽、竄改或訊息偽造的方式來通訊。 DTLS 通訊協定會以傳輸層安全性 (TLS) 通訊協定為基礎並提供對等的安全性保證，減少使用 IPsec 或設計自訂應用程式層安全性通訊協定的需要。

**這會新增什麼值？**

資料包在串流媒體中很常見，例如遊戲或安全的視訊會議。 將 DTLS 通訊協定新增到安全通道提供者，讓您能夠使用熟悉的 Windows SSPI 模型，來保護用戶端電腦與伺服器之間通訊的安全。 DTLS 特別設計為儘可能與 TLS 類似，這兩者都會盡量減少新的安全性發明，並將重新使用程式碼和基礎結構的次數最大化。

**運作方式**

透過 UDP 使用 DTLS 的應用程式可以使用 Windows Server 2012 和 Windows 8 中的 SSPI 模型。 特定的加密套件可以用來進行設定，與您設定 TLS 的方式類似。 安全通道會繼續使用 CNG 密碼編譯提供者以利用 FIPS 140 憑證，此憑證是在 Windows Vista 中所引進。

### <a name="deprecated-functionality"></a><a name="BKMK_Deprecated"></a>已被取代的功能
在 Windows Server 2012 和 Windows 8 的安全通道 SSP 中，沒有已淘汰的功能或功能。

## <a name="see-also"></a>另請參閱
-   [私人雲端安全性模型 - 包裝函式功能](https://social.technet.microsoft.com/wiki/contents/articles/6756.private-cloud-security-model-wrapper-functionality.aspx)



---
title: 安全性支援提供者介面架構
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de09e099-5711-48f8-adbd-e7b8093a0336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 8b0a74089c5d8a88a380f8a56e3b9e03b84081c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827019"
---
# <a name="security-support-provider-interface-architecture"></a>安全性支援提供者介面架構

>適用於：Windows Server （半年通道），Windows Server 2016

這個參考主題適合 IT 專業人員說明安全性支援提供者介面 (SSPI) 架構中所使用的 Windows 驗證通訊協定。

Microsoft 安全性支援提供者介面 (SSPI) 是 Windows 驗證的基礎。 應用程式和基礎結構服務需要驗證使用 SSPI 提供它。

SSPI 是實作的一般安全性服務 API (GSSAPI) 在 Windows Server 作業系統。 如需 GSSAPI 的詳細資訊，請參閱 RFC 2743 和 RFC 2744 IETF RFC 資料庫中。

預設安全性支援提供者 (Ssp)，以叫用特定的驗證通訊協定，在 Windows 中併入 SSPI 作為 Dll。 這些預設 Ssp 是由下列各節所述。 如果它們可以作業使用 SSPI 時，會合併其他 Ssp。

如下圖所示，在 Windows SSPI 提供驗證權杖延續現有用戶端電腦與伺服器之間的通訊通道的機制。 當兩個電腦或裝置需要進行驗證，以便它們可以安全地通訊時，驗證的要求會路由傳送至 SSPI 完成驗證程序，不論目前使用中的網路通訊協定。 SSPI 傳回透明的二進位大型物件。 這些會在應用程式，此時可以將它們傳遞至 SSPI 層之間傳遞。 因此，SSPI 可讓應用程式使用電腦或網路上可用的各種安全性模型，而不必變更安全性系統的介面。

![圖表，顯示安全性支援提供者介面架構](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

下列各節說明使用 SSPI 互動預設 Ssp。 Ssp 可在不同的方式，在 Windows 作業系統升級的不安全的網路環境中的安全通訊。

-   [Kerberos 安全性支援提供者](#BKMK_KerbSSP)

-   [NTLM 安全性支援提供者](#BKMK_NTLMSSP)

-   [摘要安全性支援提供者](#BKMK_DigestSSP)

-   [安全通道安全性支援提供者](#BKMK_SchannelSSP)

-   [交涉安全性支援提供者](#BKMK_NegoSSP)

-   [認證安全性支援提供者](#BKMK_CredSSP)

-   [交涉擴充功能安全性支援提供者](#BKMK_NegoExtsSSP)

-   [PKU2U 安全性支援提供者](#BKMK_PKU2USSP)

本主題也包含：

[安全性支援提供者選取項目](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="BKMK_KerbSSP"></a>Kerberos 安全性支援提供者
由 Microsoft 實作，此 SSP 會使用只能使用 Kerberos 版本 5 通訊協定。 此通訊協定為基礎的網路使用群組的 RFC 4120 和草稿修訂。 它是業界標準通訊協定使用密碼或智慧卡用於互動式登入。 它也是在 Windows 中的服務慣用的驗證方法。

Kerberos 通訊協定已自 Windows 2000 的預設驗證通訊協定，因為所有的網域服務都支援 Kerberos ssp。 這些服務包括：

-   使用輕量型目錄存取通訊協定 (LDAP) 的 active Directory 查詢

-   使用遠端程序呼叫服務的遠端伺服器或工作站管理

-   列印服務

-   用戶端-伺服器驗證

-   使用伺服器訊息區塊 (SMB) 通訊協定 （也稱為 Common Internet File System 或 CIFS） 存取遠端檔案

-   分散式的檔案系統管理和轉介

-   內部網路驗證網際網路資訊服務 (IIS)

-   網際網路通訊協定安全性 (IPsec) 的安全性授權單位驗證

-   以網域使用者和電腦的 Active Directory 憑證服務的憑證要求

位置： %windir%\Windows\System32\kerberos.dll

中指定版本中預設會包含此提供者**適用於**開頭的這個主題，以及 Windows Server 2003 和 Windows XP 的清單。

**Kerberos 通訊協定和 Kerberos SSP 的其他資源**

-   [Microsoft Kerberos (Windows)](https://msdn.microsoft.com/library/aa378747(VS.85).aspx)

-   [\[MS KILE\]:Kerberos 通訊協定延伸模組](https://msdn.microsoft.com/library/cc233855(PROT.10).aspx)

-   [\[MS-SFU\]:Kerberos 通訊協定延伸：Service for User 與限制的委派通訊協定規格](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)

-   [Kerberos SSP/AP (Windows)](https://msdn.microsoft.com/library/aa377942(VS.85).aspx)

-   [Kerberos 增強功能](https://technet.microsoft.com/library/cc749438(v=ws.10).aspx)適用於 Windows Vista

-   [Kerberos 驗證的變更](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx)for Windows 7 

-   [Kerberos 驗證技術參考](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)

### <a name="BKMK_NTLMSSP"></a>NTLM 安全性支援提供者
NTLM 安全性支援提供者 (NTLM SSP) 是傳訊通訊協定使用的安全性支援提供者介面 (SSPI)，以允許 NTLM 挑戰回應驗證並交涉完整性與機密性選項的二進位檔。 只要使用 SSPI 驗證時，包括伺服器訊息區塊或 CIFS 驗證、 HTTP 交涉驗證 （例如，網際網路 Web 驗證），以及遠端程序呼叫服務時，會使用 NTLM。 NTLM SSP 包含 NTLM 以及 NTLM 版本 2 (NTLMv2) 驗證通訊協定。

支援的 Windows 作業系統可以使用 NTLM SSP，如下列：

-   用戶端/伺服器驗證

-   列印服務

-   使用 CIFS (SMB) 檔案存取權

-   安全的遠端程序呼叫服務或 DCOM 服務

Location: %windir%\Windows\System32\msv1_0.dll

中指定版本中預設會包含此提供者**適用於**開頭的這個主題，以及 Windows Server 2003 和 Windows XP 的清單。

**NTLM 通訊協定與 NTLM SSP 的其他資源**

-   [MSV1_0 驗證封裝 (Windows)](https://msdn.microsoft.com/library/aa378753(VS.85).aspx)

-   [NTLM 驗證的變更](https://technet.microsoft.com/library/dd566199(v=ws.10).aspx)在 Windows 7 

-   [Microsoft NTLM (Windows)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)

-   [稽核及限制 NTLM 使用量指南](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

### <a name="BKMK_DigestSSP"></a>摘要安全性支援提供者
摘要式驗證是一種業界標準，使用於輕量型目錄存取通訊協定 (LDAP) 和 web 驗證。 摘要式驗證 MD5 雜湊或訊息摘要為網路上傳輸的認證。

摘要 SSP (Wdigest.dll) 會使用下列：

-   Internet Explorer 和 Internet Information Services (IIS) 的存取

-   LDAP 查詢

位置： %windir%\Windows\System32\Digest.dll

中指定版本中預設會包含此提供者**適用於**開頭的這個主題，以及 Windows Server 2003 和 Windows XP 的清單。

**摘要通訊協定和摘要 SSP 的其他資源**

-   [Microsoft 摘要式驗證 (Windows)](https://msdn.microsoft.com/library/aa378745(VS.85).aspx)

-   [\[MS DPSP\]:摘要通訊協定延伸模組](https://msdn.microsoft.com/library/cc227906(PROT.13).aspx)

### <a name="BKMK_SchannelSSP"></a>安全通道安全性支援提供者
安全通道 (Schannel) 用於 web 架構的伺服器驗證，例如當使用者嘗試存取安全的 web 伺服器。

TLS 通訊協定、 SSL 通訊協定、 私用通訊技術 (PCT) 通訊協定和資料包傳輸層 (DTLS) 通訊協定以公開金鑰密碼編譯。 安全通道會提供所有這些通訊協定。 所有的安全通道通訊協定都使用用戶端/伺服器模型。 安全通道 SSP 會使用公開金鑰憑證來驗證合作對象。 當驗證合作對象，安全通道 SSP 會依下列順序的喜好設定選取通訊協定：

-   傳輸層安全性 (TLS) 1.0 版

-   傳輸層安全性 (TLS) 1.1 版

-   傳輸層安全性 (TLS) 1.2 版

-   安全通訊端層 (SSL) 2.0 版

-   安全通訊端層 (SSL) 3.0 版

-   私有通訊技術 (PCT)

    **請注意**預設會停用 PCT。

已選取的通訊協定是用戶端和伺服器支援的慣用的驗證通訊協定。 比方說，如果伺服器支援所有的 Schannel 通訊協定和用戶端僅支援 SSL 3.0 和 SSL 2.0，在驗證程序會使用 SSL 3.0。

應用程式明確呼叫時，會使用 DTLS。 如需 DTLS 和其他安全通道提供者所使用的通訊協定的詳細資訊，請參閱 <<c0> [ 安全通道安全性支援提供者技術參考](../tls/schannel-security-support-provider-technical-reference.md)。

位置： %windir%\Windows\System32\Schannel.dll

中指定版本中預設會包含此提供者**適用於**開頭的這個主題，以及 Windows Server 2003 和 Windows XP 的清單。

> [!NOTE]
> 此提供者，在 Windows Server 2008 R2 和 Windows 7 中引進了 TLS 1.2。 DTLS 是在此提供者，Windows Server 2012 和 Windows 8 中引進。

**TLS 和 SSL 通訊協定和安全通道 SSP 的其他資源**

-   [安全通道 (Windows)](https://msdn.microsoft.com/library/aa380123(VS.85).aspx)

-   [TLS/SSL 技術參考](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)

-   [\[MS TLSP\]:傳輸層安全性 (TLS) 設定檔](https://msdn.microsoft.com/library/dd207968(PROT.13).aspx)

### <a name="BKMK_NegoSSP"></a>交涉安全性支援提供者
Simple and Protected GSS-API Negotiation Mechanism (SPNEGO) 交涉 ssp 中，會形成基礎 whichcan 用來交涉特定驗證通訊協定。 當應用程式呼叫 sspi 以登入網路時，它可以指定 SSP 來處理要求。 如果應用程式指定交涉 SSP，它會分析要求，並挑選適當的提供者來處理要求，根據客戶設定的安全性原則。

SPNEGO 是以 RFC 2478 指定。

支援新版 Windows 作業系統的詳細資訊，Negotiate 安全性支援提供者選取 Kerberos 通訊協定與 NTLM 之間。 交涉選取 Kerberos 通訊協定依預設，除非該通訊協定不能由參與驗證系統的其中一個，或呼叫應用程式無法提供足夠的資訊來使用 Kerberos 通訊協定。

位置： %windir%\Windows\System32\lsasrv.dll

中指定版本中預設會包含此提供者**適用於**開頭的這個主題，以及 Windows Server 2003 和 Windows XP 的清單。

**交涉 ssp 的其他資源**

-   [Microsoft 交涉 (Windows)](https://msdn.microsoft.com/library/aa378748(VS.85).aspx)

-   [\[MS SPNG\]:簡單和 Protected GSS-API 交涉 (SPNEGO) 機制延伸模組](https://msdn.microsoft.com/library/cc247021(PROT.13).aspx)

-   [\[MS N2HT\]:交涉和 Nego2 HTTP 驗證通訊協定規格](https://msdn.microsoft.com/library/dd303576(PROT.13).aspx)

### <a name="BKMK_CredSSP"></a>認證安全性支援提供者
啟動新的終端機服務和遠端桌面服務工作階段時，認證安全性服務提供者 (CredSSP) 會提供單一登入 (SSO) 使用者體驗。 CredSSP 會讓使用者的認證從用戶端電腦 （藉由使用委派的用戶端 SSP） 的應用程式，到目標伺服器 （透過伺服器端 SSP)，根據用戶端的原則。 使用群組原則設定 CredSSP 原則和預設關閉認證的委派。

位置： %windir%\Windows\System32\credssp.dll

中指定版本中預設會包含此提供者**適用於**本主題開頭的清單。

**認證 SSP 的其他資源**

-   [\[MS CSSP\]:認證安全性支援提供者 (CredSSP) 通訊協定規格](https://msdn.microsoft.com/library/cc226764(PROT.13).aspx)

-   [終端機認證安全性服務提供者和 SSO 服務登入](https://technet.microsoft.com/library/cc749211(v=ws.10).aspx)

### <a name="BKMK_NegoExtsSSP"></a>交涉擴充功能安全性支援提供者
交涉延伸模組 (NegoExts) 則是會交涉 Ssp，NTLM 或 Kerberos 通訊協定以外使用的驗證封裝，應用程式和案例實作 Microsoft 和其他軟體公司。

此擴充功能交涉式封裝以允許下列案例︰

-   **豐富型用戶端內同盟系統的可用性。** 可存取 SharePoint 網站上的文件，並利用功能完整的 Microsoft Office 應用程式進行編輯。

-   **豐富型用戶端支援 Microsoft Office 服務。** 使用者可以登入 Microsoft Office 服務，並使用完整的 Microsoft Office 應用程式。

-   **託管的 Microsoft Exchange Server 和 Outlook。** 沒有任何網域信任關係，因為 Exchange 伺服器裝載在網站上建立。 Outlook 會使用 Windows Live 服務來驗證使用者。

-   **用戶端電腦與伺服器之間的豐富型用戶端可用性。** 會使用作業系統的網路和驗證元件。

Windows 交涉封裝的相同方式處理 NegoExts SSP，如同針對 Kerberos 和 NTLM。 NegoExts.dll 會載入到本機系統授權 (LSA) 在啟動時。 收到的驗證要求時，根據要求的來源，則會將 NegoExts 支援的 Ssp 之間進行交涉。 它會收集的認證和原則，加密，並將該資訊傳送到適當的 SSP，建立安全性權杖的位置。

支援 NegoExts Ssp 不是獨立的 Ssp，例如 Kerberos 和 NTLM。 因此，NegoExts SSP 中, 時的驗證方法失敗，因為任何原因而驗證失敗的訊息會顯示或記錄。 可能不會有任何重新交涉或後援驗證方法。

位置： %windir%\Windows\System32\negoexts.dll

中指定版本中預設會包含此提供者**適用於**本主題中，Windows Server 2008 和 Windows Vista，但不包括開頭的清單。

### <a name="BKMK_PKU2USSP"></a>PKU2U 安全性支援提供者
PKU2U 通訊協定是引進，並實作為 Windows 7 和 Windows Server 2008 R2 中的 SSP。 此 SSP 可讓端對端驗證，特別是透過媒體和檔案共用功能，稱為 家用群組，Windows 7 中引進。 此功能可讓不是網域成員電腦之間共用。

位置： %windir%\Windows\System32\pku2u.dll

中指定版本中預設會包含此提供者**適用於**本主題中，Windows Server 2008 和 Windows Vista，但不包括開頭的清單。

**PKU2U 通訊協定和 PKU2U SSP 的其他資源**

-   [線上身分識別整合簡介](https://technet.microsoft.com/library/dd560662(v=ws.10).aspx)

## <a name="BKMK_SecuritySupportProviderSelection"></a>安全性支援提供者選取項目
Windows SSPI 可以使用任何已安裝的安全性支援提供者可支援的通訊協定。 不過，因為並非所有作業系統都支援相同的 SSP 封裝為任何指定的電腦執行 Windows Server，則用戶端和伺服器必須交涉要使用的通訊協定，兩者都支援。 Windows Server 偏好的用戶端電腦和應用程式時，但作業系統會繼續，讓用戶端電腦和用戶端不支援 Kerberos 的應用程式使用 Kerberos 通訊協定，強式的標準通訊協定，若要驗證的通訊協定。

驗證可能需要進行兩個通訊之前，電腦必須同意通訊協定，它們都可以支援。 可透過 SSPI 的任何通訊協定，每部電腦都必須有適當的 ssp。 例如，如需用戶端電腦和伺服器使用 Kerberos 驗證通訊協定，它們必須都支援 Kerberos v5。 Windows Server 會使用函式**EnumerateSecurityPackages**來識別電腦和項目上支援的 Ssp 是那些 Ssp 的功能。

在下列兩種方式之一，可以處理的驗證通訊協定選取項目：

1.  [單一的驗證通訊協定](#BKMK_SingleAuth)

2.  [交涉選項](#BKMK_Negotiate)

### <a name="BKMK_SingleAuth"></a>單一的驗證通訊協定
在伺服器上指定的單一可接受的通訊協定時，用戶端電腦必須支援指定的通訊協定或通訊失敗。 指定單一的可接受通訊協定時，驗證交換，如下所示發生：

1.  用戶端電腦會要求服務的存取權。

2.  伺服器回覆的要求，並指定將使用的通訊協定。

3.  用戶端電腦會檢查回覆和檢查，以判斷它是否支援指定的通訊協定的內容。 如果用戶端電腦支援指定的通訊協定，會繼續驗證。 如果用戶端電腦不支援的通訊協定，則驗證會失敗，不論是否授權用戶端電腦存取資源。

### <a name="BKMK_Negotiate"></a>交涉選項
Negotiate 選項可用來允許用戶端和伺服器，以嘗試尋找可接受的通訊協定。 這根據 Simple and Protected GSS-API Negotiation Mechanism (SPNEGO)。 驗證開始時的驗證通訊協定交涉的選項，連接 SPNEGO 交換會進行，如下所示：

1.  用戶端電腦會要求服務的存取權。

2.  伺服器回覆的驗證通訊協定，它可以支援的驗證挑戰或回應，依據是其第一個選擇的通訊協定清單。 比方說，伺服器可能列出的 Kerberos 通訊協定與 NTLM 和 Kerberos 驗證回應傳送。

3.  用戶端電腦會檢查回覆和檢查，以判斷它是否支援任何指定的通訊協定的內容。

    -   如果用戶端電腦支援的慣用的通訊協定，將會繼續驗證。

    -   如果用戶端電腦不支援的慣用的通訊協定，但它確實支援依伺服器列出的其他通訊協定之一，用戶端電腦可讓伺服器知道哪些它支援的驗證通訊協定和驗證會繼續。

    -   如果用戶端電腦不支援任何列出的通訊協定，驗證交換將會失敗。

## <a name="see-also"></a>另請參閱
[Windows 驗證架構](https://technet.microsoft.com/library/dn169024(v=ws.10).aspx)



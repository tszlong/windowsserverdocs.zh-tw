---
title: "安全性支援提供者介面架構"
description: "Windows Server 安全性"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="security-support-provider-interface-architecture"></a>安全性支援提供者介面架構

>適用於：Windows Server（以每年次管道）、Windows Server 2016

這適用於 IT 專業人員的參考主題描述架構安全性支援提供者介面 (SSPI) 中可用的 Windows 驗證通訊協定。

Microsoft Security 支援提供者介面 (SSPI) 是 Windows 驗證的基礎配置。 應用程式和需要驗證的基礎結構服務使用 SSPI 提供。

SSPI 是實作的一般安全性服務 API (GSSAPI) 在 Windows Server 作業系統。 如需 GSSAPI，查看 RFC 2743 和 RFC 2744 IETF RFC 資料庫中。

預設安全性支援的提供者 （層） 叫用在 Windows 中的特定驗證通訊協定，會納入為 Dll SSPI。 這些預設層所述的下列區段。 如果他們使用 SSPI 可以運作，就可以加入層額外。

在下列影像中所示，在 Windows 中的 SSPI 提供帶來透過現有的通訊通道 client 電腦之間伺服器的驗證權杖機制。 兩部電腦或裝置需要時進行驗證，讓他們可以通訊，要求驗證都會通過，SSPI，完成驗證程序，無論使用目前的網路通訊協定。 SSPI 傳回透明二進位大物件。 這些應用程式，此時他們可以傳遞至 SSPI 層之間傳送。 因此，SSPI 可讓應用程式提供各種不同的安全性型號使用電腦或網路上，而無須更動介面安全性系統。

![圖表顯示安全性支援提供者介面架構](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

下列章節描述與 SSPI 互動預設層。 層的宣傳安全通訊在安全的網路的環境中使用 Windows 作業系統以不同方式。

-   [Kerberos 安全性支援提供者](#BKMK_KerbSSP)

-   [以滑鼠](#BKMK_NTLMSSP)

-   [摘要安全性支援提供者](#BKMK_DigestSSP)

-   [Schannel 安全性支援提供者](#BKMK_SchannelSSP)

-   [交涉安全性支援提供者](#BKMK_NegoSSP)

-   [認證安全性支援提供者](#BKMK_CredSSP)

-   [交涉擴充功能安全性支援提供者](#BKMK_NegoExtsSSP)

-   [PKU2U 安全性支援提供者](#BKMK_PKU2USSP)

此主題也包含：

[安全性支援提供者選取項目](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="BKMK_KerbSSP"></a>Kerberos 安全性支援提供者
這個 SSP 使用只 Kerberos 版本 5 通訊協定實作 microsoft。 此通訊協定根據網路運作群組 RFC 4120 和草稿修訂。 它是使用密碼或智慧卡用於互動式登入業界標準通訊協定。 這也是 Windows 中的服務慣用的驗證方法。

所有網域服務 Kerberos 通訊協定已從 Windows 2000 預設驗證通訊協定，因為都支援 Kerberos SSP. 這些服務包括：

-   Active Directory 查詢使用的輕量型 Directory 存取通訊協定 (LDAP)

-   遠端伺服器或工作站管理，使用遠端程序呼叫服務

-   列印服務

-   Client 伺服器驗證

-   使用伺服器訊息區 (SMB) 通訊協定 （也稱為一般網際網路檔案系統或 CIFS） 的檔案遠端存取

-   分散式的檔案系統管理和推薦

-   內部驗證網際網路資訊服務 (IIS)

-   安全性授權單位驗證的 「 網際網路通訊協定的安全性 (IPsec)

-   Active Directory 憑證服務使用者網域和電腦的憑證要求

位置： %windir%\Windows\System32\kerberos.dll

這提供者中指定的版本中的預設包含**適用於**清單此主題，以及 Windows Server 2003 及 Windows XP 的開頭。

**其他 Kerberos 通訊協定和 Kerberos SSP 資源**

-   [Microsoft Kerberos (Windows)](https://msdn.microsoft.com/library/aa378747(VS.85).aspx)

-   [\[MS-KILE\]: Kerberos 通訊協定的擴充功能](https://msdn.microsoft.com/library/cc233855(PROT.10).aspx)

-   [\[MS-SFU\]: Kerberos 通訊協定的擴充功能： 使用者和限制的委派通訊協定規格服務](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)

-   [Kerberos SSP 日 AP (Windows)](https://msdn.microsoft.com/library/aa377942(VS.85).aspx)

-   [Kerberos 調節](https://technet.microsoft.com/library/cc749438(v=ws.10).aspx)適用於 Windows Vista

-   [變更 Kerberos 驗證](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx)適用於 Windows 7 

-   [Kerberos 驗證技術參考](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)

### <a name="BKMK_NTLMSSP"></a>以滑鼠
以滑鼠 (NTLM SSP) 是二進位訊息通訊協定，允許 NTLM 挑戰回應驗證和商議完整性和機密性選項用來安全性支援提供者介面 (SSPI)。 只要使用 SSPI 驗證，驗證伺服器訊息區或 CIFS、 HTTP 交涉驗證 （例如，網際網路 Web 驗證），和遠端程序呼叫服務包括 NTLM 使用。 NTLM SSP 包括 NTLM 和 NTLM 版本 (NTLMv2) 2 驗證通訊協定。

支援的 Windows 作業系統可以使用下列的 NTLM SSP:

-   Client 日伺服器驗證

-   列印服務

-   使用 CIFS (SMB) 的存取檔案

-   安全遠端程序呼叫服務或 DCOM 服務

位置： %windir%\Windows\System32\msv1_0.dll

這提供者中指定的版本中的預設包含**適用於**清單此主題，以及 Windows Server 2003 及 Windows XP 的開頭。

**其他 NTLM 通訊協定和 NTLM SSP 資源**

-   [MSV1_0 驗證套件 (Windows)](https://msdn.microsoft.com/library/aa378753(VS.85).aspx)

-   [變更 NTLM 驗證](https://technet.microsoft.com/library/dd566199(v=ws.10).aspx)在 Windows 7 中 

-   [Microsoft NTLM (Windows)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)

-   [稽核和限制 NTLM 使用指南](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

### <a name="BKMK_DigestSSP"></a>摘要安全性支援提供者
摘要驗證是適用於輕量型 Directory 存取通訊協定 (LDAP) 和 web 驗證業界標準。 摘要驗證會 MD5 hash 或郵件摘要為傳輸在網路上的憑證。

使用下列摘要 SSP (Wdigest.dll):

-   Internet Explorer 和網際網路服務 (IIS) 存取

-   LDAP 查詢

位置： %windir%\Windows\System32\Digest.dll

這提供者中指定的版本中的預設包含**適用於**清單此主題，以及 Windows Server 2003 及 Windows XP 的開頭。

**其他摘要通訊協定和摘要 SSP 資源**

-   [Microsoft 摘要驗證 (Windows)](https://msdn.microsoft.com/library/aa378745(VS.85).aspx)

-   [\[MS-DPSP\]: 摘要通訊協定的擴充功能](https://msdn.microsoft.com/library/cc227906(PROT.13).aspx)

### <a name="BKMK_SchannelSSP"></a>Schannel 安全性支援提供者
Web 架構伺服器的驗證，例如使用者嘗試存取安全的網頁伺服器使用最安全的通道 (Schannel)。

TLS 通訊協定、 SSL 通訊協定，私人通訊技術 (PCT) 通訊協定及資料流傳輸層 (DTLS) 通訊協定根據公開加密。 Schannel 提供所有的這些通訊協定。 所有 Schannel 通訊協定都使用 client/伺服器的模型。 Schannel SSP 使用公開金鑰憑證，來驗證派對。 驗證派對、 時 Schannel SSP 選取通訊協定以下列順序的喜好設定：

-   運送層安全性 (TLS) 1.0

-   運送層安全性 (TLS) 1.1 版

-   運送層安全性 (TLS) 1.2 版

-   安全通訊端層 (SSL) 2.0

-   安全通訊端層 (SSL) 版本 3.0

-   私人通訊技術 (PCT)

    **注意**PCT 預設停用。

通訊協定，選取 [是，可支援 client 和 server 慣用的驗證通訊協定。 例如，如果伺服器支援所有的 Schannel 通訊協定，client 支援僅 SSL 3.0 和 SSL 2.0 驗證程序使用 SSL 3.0。

DTLS 明確應用程式呼叫時使用。 如需有關 DTLS Schannel 提供者所使用的其他通訊協定，請[Schannel 安全性支援提供者技術參考](../tls/schannel-security-support-provider-technical-reference.md)。

位置： %windir%\Windows\System32\Schannel.dll

這提供者中指定的版本中的預設包含**適用於**清單此主題，以及 Windows Server 2003 及 Windows XP 的開頭。

> [!NOTE]
> 在此提供者，在 Windows Server 2008 R2 和 Windows 7 中引進 TLS 1.2。 在此提供者，在 Windows Server 2012 和 Windows 8 中引進 DTLS。

**其他的 TLS 和 SSL 通訊協定和 Schannel SSP 資源**

-   [安全的通道 (Windows)](https://msdn.microsoft.com/library/aa380123(VS.85).aspx)

-   [TLS 日 SSL 技術參考](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)

-   [\[MS-TLSP\]: 傳輸層 (TLS) 的安全性設定檔](https://msdn.microsoft.com/library/dd207968(PROT.13).aspx)

### <a name="BKMK_NegoSSP"></a>交涉安全性支援提供者
簡單、 保護 GSS-API 交涉機制 (SPNEGO) 形成基礎交涉 ssp，whichcan 用於交涉特定驗證通訊協定。 當應用程式呼叫到 SSPI 登入網路時，它可以指定 SSP 處理要求。 如果應用程式指定交涉 SSP，它會分析要求，並選取適當的提供者來處理要求，根據客戶設定的安全性原則。

指定 SPNEGO RFC 2478 中。

支援的 Windows 作業系統版本、 交涉安全性支援 Kerberos 通訊協定與 NTLM 之間的提供者選取。 交涉選取預設通訊協定 Kerberos，除非該通訊協定無法使用其中一個參與驗證、 系統或通話的應用程式並未提供的資訊用於 Kerberos 通訊協定不足。

位置： %windir%\Windows\System32\lsasrv.dll

這提供者中指定的版本中的預設包含**適用於**清單此主題，以及 Windows Server 2003 及 Windows XP 的開頭。

**其他交涉 SSP 資源**

-   [Microsoft 交涉 (Windows)](https://msdn.microsoft.com/library/aa378748(VS.85).aspx)

-   [\[MS-SPNG\]: 簡單和受保護的 GSS API 交涉機制 (SPNEGO) 擴充功能](https://msdn.microsoft.com/library/cc247021(PROT.13).aspx)

-   [\[MS-N2HT\]: 議和 Nego2 HTTP 驗證通訊協定規格](https://msdn.microsoft.com/library/dd303576(PROT.13).aspx)

### <a name="BKMK_CredSSP"></a>認證安全性支援提供者
開始新的車票服務和遠端桌面服務工作階段時，認證安全性服務提供者 (CredSSP) 提供單一登入 (SSO) 的使用者體驗。 CredSSP 可以根據原則 client 的目標伺服器 （透過伺服器端 SSP)，讓應用程式的 client 電腦的使用者的認證委派 （透過 client 端 SSP）。 CredSSP 原則，使用群組原則、 設定和的認證委派預設為關閉。

位置： %windir%\Windows\System32\credssp.dll

這提供者中指定的版本中的預設包含**適用於**清單中的開頭本主題。

**其他認證 SSP 資源**

-   [\[MS-CSSP\]: credential 安全性支援提供者 (CredSSP) 通訊協定規格](https://msdn.microsoft.com/library/cc226764(PROT.13).aspx)

-   [Credential 安全性服務提供者和 SSO 車票的服務登入](https://technet.microsoft.com/library/cc749211(v=ws.10).aspx)

### <a name="BKMK_NegoExtsSSP"></a>交涉擴充功能安全性支援提供者
交涉擴充功能 (NegoExts) 是驗證套件，交涉層，以外 NTLM 或 Kerberos 通訊協定，使用的應用程式和案例實作 Microsoft 或其他軟體的公司。

這個交涉套件延伸模組允許下列案例：

-   **豐富 client 可用性聯盟系統中。** 可以存取 SharePoint 網站上的文件，並使用完整的 Microsoft Office 應用程式可以編輯它們。

-   **Microsoft Office 服務豐富 client 支援。** 使用者可以登入 Microsoft Office 服務，並使用完整的 Microsoft Office 應用程式。

-   **裝載的 Microsoft Exchange Server 與 Outlook。** 還有不因為 Exchange Server 裝載在網路上建立的網域信任。 Outlook 驗證使用者使用 Windows Live 服務。

-   **豐富 client 可用性 client 電腦之間的伺服器。** 使用作業系統的網路與驗證的元件。

Windows 交涉套件會以相同的方式將 NegoExts SSP Kerberos 和 NTLM 一樣。 NegoExts.dll 載入到本機系統授權 (LSA) 在開機。 當您收到的驗證要求，以要求的來源，NegoExts 交涉之間支援層。 它也會收集認證原則，已加密，並將資訊傳送至適當 SSP，建立的安全性權杖的位置。

支援 NegoExts 層的獨立層 Kerberos 和 NTLM。 因此，NegoExts SSP，在任何原因而失敗的驗證方法時驗證失敗的訊息會顯示或登入。 可能會無交涉或後援驗證方法。

位置： %windir%\Windows\System32\negoexts.dll

這提供者中指定的版本中的預設包含**適用於**清單本主題中，不包含 Windows Server 2008 和 Windows Vista 的開頭。

### <a name="BKMK_PKU2USSP"></a>PKU2U 安全性支援提供者
導入了 PKU2U 通訊協定，以在 Windows 7 和 Windows Server 2008 R2 SSP 實作。 這個 SSP 可讓您對等驗證，尤其是透過媒體和檔案共用稱為 [在 Windows 7 家用群組的功能。 此功能可以讓您不是成員加入網域的電腦之間共用。

位置： %windir%\Windows\System32\pku2u.dll

這提供者中指定的版本中的預設包含**適用於**清單本主題中，不包含 Windows Server 2008 和 Windows Vista 的開頭。

**其他 PKU2U 通訊協定和 PKU2U SSP 資源**

-   [簡介 Online 身分整合](https://technet.microsoft.com/library/dd560662(v=ws.10).aspx)

## <a name="BKMK_SecuritySupportProviderSelection"></a>安全性支援提供者選取項目
Windows SSPI 可使用任何支援透過安裝安全性支援提供者通訊協定。 不過，並非所有的作業系統支援為任何特定的電腦執行的 Windows Server 的相同 SSP 套件，因為戶端與伺服器必須協議出使用這兩個他們支援的通訊協定。 Windows Server 慣用 client 電腦及時，不過在作業系統持續允許 client 電腦及 client 不支援 Kerberos 通訊協定進行驗證的應用程式使用 Kerberos 通訊協定，穩固標準通訊協定，應用程式。

驗證可能需要兩個位置聯繫之前，電腦必須同意通訊協定，它們都可以支援。 若要透過 SSPI 可用的任何通訊協定，每個的電腦必須具備適當 SSP. 例如，client 電腦和伺服器使用 Kerberos 驗證通訊協定，他們必須同時支援 Kerberos v5。 Windows Server 使用函式**EnumerateSecurityPackages**這些層的功能是哪一個層支援的電腦，以及找出。

在下列兩種方式可處理驗證通訊協定的選取項目：

1.  [單一驗證通訊協定](#BKMK_SingleAuth)

2.  [交涉選項](#BKMK_Negotiate)

### <a name="BKMK_SingleAuth"></a>單一驗證通訊協定
在伺服器上指定的單一接受通訊協定時，電腦 client 必須支援指定的通訊協定或在通訊失敗。 指定單一接受通訊協定時，驗證交換進行時，如下所示：

1.  Client 電腦要求服務的存取。

2.  伺服器回覆要求，指定的通訊協定，將會使用。

3.  Client 的電腦檢查回覆及檢查以判斷它是否支援指定的通訊協定。 如果 client 電腦支援指定的通訊協定，繼續進行驗證。 如果 client 的電腦不支援的通訊協定，驗證失敗，無論 client 的電腦是否獲授權存取資源。

### <a name="BKMK_Negotiate"></a>交涉選項
交涉選項可讓您嘗試尋找接受通訊協定 client 及伺服器。 這根據簡單和保護 GSS-API 交涉機制 (SPNEGO)。 當驗證開頭交涉驗證通訊協定的選項時，SPNEGO 換貨會的地方，如下所示：

1.  Client 電腦要求服務的存取。

2.  伺服器的驗證，它可支援的通訊協定驗證要求或根據其第一次選擇的通訊協定的回應清單回覆。 例如伺服器可能會列出 Kerberos 通訊協定與 NTLM，並傳送 Kerberos 驗證回覆。

3.  Client 的電腦檢查回覆及檢查以判斷它是否支援的任何指定的通訊協定。

    -   如果 client 電腦支援慣用的通訊協定，繼續進行驗證。

    -   如果 client 的電腦不支援的慣用的通訊協定，但它伺服器所列出的其他通訊協定的其中一個支援，請 client 電腦讓知道進行驗證，支援的通訊協定與驗證的伺服器。

    -   如果 client 的電腦不支援的任何列出的通訊協定，驗證交換將會失敗。

## <a name="see-also"></a>也了
[Windows 驗證架構](https://technet.microsoft.com/library/dn169024(v=ws.10).aspx)



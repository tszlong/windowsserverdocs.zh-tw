---
title: 安全性支援提供者介面架構
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4db407b24b00bc8313d2e17f1fcf55d9fa160c8c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403302"
---
# <a name="security-support-provider-interface-architecture"></a>安全性支援提供者介面架構

>適用於：Windows Server (半年通道)、Windows Server 2016

這個適用于 IT 專業人員的參考主題說明安全性支援提供者介面（SSPI）架構中使用的 Windows 驗證通訊協定。

Microsoft 安全性支援提供者介面（SSPI）是 Windows 驗證的基礎。 需要驗證的應用程式和基礎結構服務使用 SSPI 來提供它。

SSPI 是 Windows Server 作業系統中的一般安全性服務 API （GSSAPI）的執行。 如需 GSSAPI 的詳細資訊，請參閱 IETF RFC 資料庫中的 RFC 2743 和 RFC 2744。

在 Windows 中叫用特定驗證通訊協定的預設安全性支援提供者（Ssp），會併入 SSPI 做為 Dll。 下列各節將說明這些預設的 Ssp。 如果可以與 SSPI 進行操作，可以合併其他 Ssp。

如下圖所示，Windows 中的 SSPI 提供了一種機制，可在用戶端電腦與伺服器之間的現有通道上攜帶驗證權杖。 當兩部電腦或裝置必須經過驗證才能安全地進行通訊時，驗證的要求會路由傳送到 SSPI，這會完成驗證程式，而不論目前使用的網路通訊協定為何。 SSPI 會傳回透明的二進位大型物件。 這些會在應用程式之間傳遞，此時可以將它們傳遞給 SSPI 層。 因此，SSPI 可以讓應用程式在電腦或網路上使用各種可用的安全性模型，而不需要變更安全性系統的介面。

![顯示安全性支援提供者介面架構的圖表](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

下列各節說明與 SSPI 互動的預設 Ssp。 在 Windows 作業系統中，會以不同的方式使用 Ssp，在不安全的網路環境中升級安全通訊。

-   [Kerberos 安全性支援提供者](#BKMK_KerbSSP)

-   [NTLM 安全性支援提供者](#BKMK_NTLMSSP)

-   [摘要式安全性支援提供者](#BKMK_DigestSSP)

-   [Schannel 安全性支援提供者](#BKMK_SchannelSSP)

-   [Negotiate 安全性支援提供者](#BKMK_NegoSSP)

-   [認證安全性支援提供者](#BKMK_CredSSP)

-   [Negotiate 延伸模組安全性支援提供者](#BKMK_NegoExtsSSP)

-   [PKU2U 安全性支援提供者](#BKMK_PKU2USSP)

本主題中也包含：

[安全性支援提供者選擇](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="BKMK_KerbSSP"></a>Kerberos 安全性支援提供者
此 SSP 只會使用 Microsoft 所實行的 Kerberos 第5版通訊協定。 此通訊協定是以網路工作群組的 RFC 4120 和草稿修訂為基礎。 這是一種業界標準通訊協定，用於互動式登入的密碼或智慧卡。 這也是 Windows 中的服務慣用的驗證方法。

因為 Kerberos 通訊協定是自 Windows 2000 之後的預設驗證通訊協定，所以所有網域服務都支援 Kerberos SSP。 這些服務包括：

-   Active Directory 使用輕量型目錄存取協定（LDAP）的查詢

-   使用遠端程序呼叫服務的遠端伺服器或工作站管理

-   列印服務

-   用戶端-伺服器驗證

-   使用伺服器訊息區（SMB）通訊協定（也稱為一般網際網路檔案系統或 CIFS）的遠端檔案存取

-   分散式檔案系統管理和參考

-   Internet Information Services 的內部網路驗證（IIS）

-   網際網路通訊協定安全性（IPsec）的安全性授權單位驗證

-   針對網域使用者和電腦 Active Directory 憑證服務的憑證要求

位置：%windir%\Windows\System32\kerberos.dll

此提供者預設包含在本主題開頭的**適用**物件清單中指定的版本，以及 windows Server 2003 和 windows XP。

**Kerberos 通訊協定和 Kerberos SSP 的其他資源**

-   [Microsoft Kerberos （Windows）](https://msdn.microsoft.com/library/aa378747(VS.85).aspx)

-   [\[MS KILE\]： Kerberos 通訊協定延伸模組](https://msdn.microsoft.com/library/cc233855(PROT.10).aspx)

-   [\[MS-CHAP\]： Kerberos 通訊協定延伸模組：適用于使用者和限制委派通訊協定規格的服務](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)

-   [Kerberos SSP/AP （Windows）](https://msdn.microsoft.com/library/aa377942(VS.85).aspx)

-   Windows Vista 的[Kerberos 增強功能](https://technet.microsoft.com/library/cc749438(v=ws.10).aspx)

-   Windows 7 的[Kerberos 驗證變更](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) 

-   [Kerberos 驗證技術參考](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)

### <a name="BKMK_NTLMSSP"></a>NTLM 安全性支援提供者
NTLM 安全性支援提供者（NTLM SSP）是一種二進位訊息通訊協定，由安全性支援提供者介面（SSPI）用來允許 NTLM 挑戰回應驗證，並協商完整性和機密性選項。 使用 SSPI 驗證時，會使用 NTLM，包括伺服器訊息區或 CIFS 驗證、HTTP Negotiate 驗證（例如，網際網路 Web 驗證），以及遠端程序呼叫服務。 NTLM SSP 包含 NTLM 和 NTLM 版本2（NTLMv2）驗證通訊協定。

支援的 Windows 作業系統可以針對下列各項使用 NTLM SSP：

-   用戶端/伺服器驗證

-   列印服務

-   使用 CIFS （SMB）進行檔案存取

-   安全遠端程序呼叫服務或 DCOM 服務

位置：%windir%\Windows\System32\ msv1_0 .dll

此提供者預設包含在本主題開頭的**適用**物件清單中指定的版本，以及 windows Server 2003 和 windows XP。

**NTLM 通訊協定和 NTLM SSP 的其他資源**

-   [MSV1_0 驗證套件（Windows）](https://msdn.microsoft.com/library/aa378753(VS.85).aspx)

-   Windows 7 中[NTLM 驗證的變更](https://technet.microsoft.com/library/dd566199(v=ws.10).aspx) 

-   [Microsoft NTLM （Windows）](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)

-   [審核和限制 NTLM 使用方式指南](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

### <a name="BKMK_DigestSSP"></a>摘要式安全性支援提供者
摘要式驗證是一種業界標準，用於輕量型目錄存取協定（LDAP）和 web 驗證。 摘要式驗證會在網路上以 MD5 雜湊或訊息摘要的方式傳輸認證。

摘要式 SSP （Wdigest .dll）用於下列各項：

-   Internet Explorer 和 Internet Information Services （IIS）存取

-   LDAP 查詢

位置：%windir%\Windows\System32\Digest.dll

此提供者預設包含在本主題開頭的**適用**物件清單中指定的版本，以及 windows Server 2003 和 windows XP。

**摘要式通訊協定和摘要 SSP 的其他資源**

-   [Microsoft Digest 驗證（Windows）](https://msdn.microsoft.com/library/aa378745(VS.85).aspx)

-   [\[DPSP\]：摘要式通訊協定延伸](https://msdn.microsoft.com/library/cc227906(PROT.13).aspx)

### <a name="BKMK_SchannelSSP"></a>Schannel 安全性支援提供者
安全通道（Schannel）用於 web 型伺服器驗證，例如當使用者嘗試存取安全的 web 伺服器時。

TLS 通訊協定、SSL 通訊協定、私人通訊技術（百分比）通訊協定，以及資料包傳輸層（DTLS）通訊協定是以公開金鑰密碼編譯為基礎。 Schannel 提供所有這些通訊協定。 所有的安全通道通訊協定都使用用戶端/伺服器模型。 安全通道 SSP 會使用公開金鑰憑證來驗證合作對象。 驗證合作物件時，Schannel SSP 會依照下列喜好順序來選取通訊協定：

-   傳輸層安全性（TLS）版本1。0

-   傳輸層安全性（TLS）版本1。1

-   傳輸層安全性（TLS）版本1。2

-   安全通訊端層（SSL）版本2。0

-   安全通訊端層（SSL）版本3。0

-   私人通訊技術（PCT）

    **注意**預設會停用 PCT。

選取的通訊協定是用戶端和伺服器可支援的慣用驗證通訊協定。 例如，如果伺服器支援所有安全通道通訊協定，而且用戶端僅支援 SSL 3.0 和 SSL 2.0，則驗證程式會使用 SSL 3.0。

應用程式明確呼叫時，會使用 DTLS。 如需安全通道提供者所使用之 DTLS 和其他通訊協定的詳細資訊，請參閱[Schannel 安全性支援提供者技術參考](../tls/schannel-security-support-provider-technical-reference.md)。

位置：%windir%\Windows\System32\Schannel.dll

此提供者預設包含在本主題開頭的**適用**物件清單中指定的版本，以及 windows Server 2003 和 windows XP。

> [!NOTE]
> TLS 1.2 是在 Windows Server 2008 R2 和 Windows 7 的此提供者中引進。 DTLS 是在 Windows Server 2012 和 Windows 8 的此提供者中引進。

**TLS 和 SSL 通訊協定和 Schannel SSP 的其他資源**

-   [安全通道（Windows）](https://msdn.microsoft.com/library/aa380123(VS.85).aspx)

-   [TLS/SSL 技術參考](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)

-   [\[MS TLSP\]：傳輸層安全性（TLS）設定檔](https://msdn.microsoft.com/library/dd207968(PROT.13).aspx)

### <a name="BKMK_NegoSSP"></a>Negotiate 安全性支援提供者
簡單且受保護的 GSS-API 協調機制（SPNEGO）會形成 Negotiate SSP 的基礎，whichcan 可用來協調特定的驗證通訊協定。 當應用程式呼叫 SSPI 來登入網路時，它可以指定 SSP 來處理要求。 如果應用程式指定 Negotiate SSP，它會分析要求，並根據客戶設定的安全性原則，挑選適當的提供者來處理要求。

SPNEGO 是在 RFC 2478 中指定。

在支援的 Windows 作業系統版本中，「協商安全性支援提供者」會在 Kerberos 通訊協定與 NTLM 之間進行選取。 根據預設，除非其中一個與驗證有關的系統無法使用該通訊協定，或呼叫應用程式未提供足夠的資訊來使用 Kerberos 通訊協定，否則 Negotiate 會選取 Kerberos 通訊協定。

位置：%windir%\Windows\System32\lsasrv.dll

此提供者預設包含在本主題開頭的**適用**物件清單中指定的版本，以及 windows Server 2003 和 windows XP。

**Negotiate SSP 的其他資源**

-   [Microsoft Negotiate （Windows）](https://msdn.microsoft.com/library/aa378748(VS.85).aspx)

-   [\[SPNG\]：簡單且受保護的 GSS-API 協調機制（SPNEGO）延伸模組](https://msdn.microsoft.com/library/cc247021(PROT.13).aspx)

-   [\[MS N2HT\]： Negotiate 和 Nego2 HTTP 驗證通訊協定規格](https://msdn.microsoft.com/library/dd303576(PROT.13).aspx)

### <a name="BKMK_CredSSP"></a>認證安全性支援提供者
認證安全性服務提供者（CredSSP）可在啟動新的終端機服務和遠端桌面服務會話時，提供單一登入（SSO）使用者體驗。 CredSSP 可讓應用程式根據用戶端的原則，將使用者的認證從用戶端電腦（藉由使用用戶端 SSP）委派給目標伺服器（透過伺服器端 SSP）。 CredSSP 原則是使用群組原則進行設定，而且預設會關閉認證的委派。

位置：%windir%\Windows\System32\credssp.dll

此提供者預設會包含在本主題開頭的**適用**物件清單中指定的版本。

**認證 SSP 的其他資源**

-   [\[MS CSSP\]：認證安全性支援提供者（CredSSP）通訊協定規格](https://msdn.microsoft.com/library/cc226764(PROT.13).aspx)

-   [適用于終端機服務登入的認證安全性服務提供者和 SSO](https://technet.microsoft.com/library/cc749211(v=ws.10).aspx)

### <a name="BKMK_NegoExtsSSP"></a>Negotiate 延伸模組安全性支援提供者
Negotiate 延伸模組（NegoExts）是一種驗證套件，可針對 Microsoft 和其他軟體公司所實行的應用程式和案例，協調 Ssp 的使用，而不是 NTLM 或 Kerberos 通訊協定。

此 Negotiate 套件的延伸模組可允許下列案例：

-   **聯合系統內豐富的用戶端可用性。** 檔可以在 SharePoint 網站上存取，而且可以使用功能完整的 Microsoft Office 應用程式進行編輯。

-   **Microsoft Office 服務的豐富用戶端支援。** 使用者可以登入 Microsoft Office 服務，並使用功能完整的 Microsoft Office 應用程式。

-   **託管 Microsoft Exchange Server 和 Outlook。** 由於 Exchange Server 裝載在 web 上，因此不會建立網域信任。 Outlook 會使用 Windows Live 服務來驗證使用者。

-   **用戶端電腦與伺服器之間的豐富用戶端可用性。** 系統會使用作業系統的網路和驗證元件。

Windows Negotiate 套件會以與 Kerberos 和 NTLM 相同的方式來處理 NegoExts 的 SSP。 NegoExts 會在啟動時載入至本機系統授權單位（LSA）。 收到驗證要求時，會根據要求的來源，NegoExts 會在支援的 Ssp 之間進行協調。 它會收集認證和原則、將其加密，並將該資訊傳送至適當的 SSP，其中會建立安全性權杖。

NegoExts 支援的 Ssp 不是獨立的 Ssp，例如 Kerberos 和 NTLM。 因此，在 NegoExts SSP 中，當驗證方法因任何原因而失敗時，將會顯示或記錄驗證失敗訊息。 不可能進行重新協商或回退驗證方法。

位置：%windir%\Windows\System32\negoexts.dll

此提供者預設會包含在本主題開頭的 [**適用于**] 清單中指定的版本中，但不包括 windows Server 2008 和 windows Vista。

### <a name="BKMK_PKU2USSP"></a>PKU2U 安全性支援提供者
PKU2U 通訊協定已引進並實作為 Windows 7 和 Windows Server 2008 R2 中的 SSP。 這個 SSP 會啟用對等驗證，特別是透過 Windows 7 中引進的「家庭功能」媒體和「檔案共用」功能。 此功能允許在不屬於網域成員的電腦之間共用。

位置：%windir%\Windows\System32\pku2u.dll

此提供者預設會包含在本主題開頭的 [**適用于**] 清單中指定的版本中，但不包括 windows Server 2008 和 windows Vista。

**PKU2U 通訊協定和 PKU2U SSP 的其他資源**

-   [線上身分識別整合簡介](https://technet.microsoft.com/library/dd560662(v=ws.10).aspx)

## <a name="BKMK_SecuritySupportProviderSelection"></a>安全性支援提供者選擇
Windows SSPI 可以使用任何透過已安裝的安全性支援提供者所支援的通訊協定。 不過，由於並非所有作業系統都支援與執行 Windows Server 的任何指定電腦相同的 SSP 套件，因此用戶端和伺服器必須協商，才能使用兩者都支援的通訊協定。 Windows Server 偏好用戶端電腦和應用程式使用 Kerberos 通訊協定，這是一種以強式標準為基礎的通訊協定，但是作業系統會繼續允許不支援 Kerberos 的用戶端電腦和用戶端應用程式。要驗證的通訊協定。

在進行驗證之前，兩部通訊電腦必須同意兩者皆可支援的通訊協定。 對於可透過 SSPI 使用的任何通訊協定，每部電腦都必須有適當的 SSP。 例如，若要讓用戶端電腦和伺服器使用 Kerberos 驗證通訊協定，它們都必須支援 Kerberos v5。 Windows Server 會使用函式**EnumerateSecurityPackages**來識別電腦上支援的 ssp，以及這些 ssp 的功能。

您可以使用下列兩種方式的其中一種來處理驗證通訊協定的選擇：

1.  [單一驗證通訊協定](#BKMK_SingleAuth)

2.  [Negotiate 選項](#BKMK_Negotiate)

### <a name="BKMK_SingleAuth"></a>單一驗證通訊協定
當伺服器上指定了單一可接受的通訊協定時，用戶端電腦必須支援指定的通訊協定或通訊失敗。 當指定了單一可接受的通訊協定時，驗證交換就會如下所示：

1.  用戶端電腦會要求服務的存取權。

2.  伺服器會回復要求，並指定將使用的通訊協定。

3.  用戶端電腦會檢查回復的內容，並檢查以判斷它是否支援指定的通訊協定。 如果用戶端電腦支援指定的通訊協定，則會繼續進行驗證。 如果用戶端電腦不支援通訊協定，則無論用戶端電腦是否有權存取資源，驗證都會失敗。

### <a name="BKMK_Negotiate"></a>Negotiate 選項
您可以使用 negotiate 選項，讓用戶端和伺服器嘗試尋找可接受的通訊協定。 這是以簡單且受保護的 GSS-API 協調機制（SPNEGO）為基礎。 當驗證開始時，會使用 negotiate 驗證通訊協定的選項，SPNEGO exchange 會如下所示：

1.  用戶端電腦會要求服務的存取權。

2.  伺服器會根據其第一個選擇的通訊協定，以可支援的驗證通訊協定清單和驗證挑戰或回應來回複。 例如，伺服器可能會列出 Kerberos 通訊協定和 NTLM，並傳送 Kerberos 驗證回應。

3.  用戶端電腦會檢查回復的內容，並檢查以判斷它是否支援任何指定的通訊協定。

    -   如果用戶端電腦支援慣用的通訊協定，則會繼續進行驗證。

    -   如果用戶端電腦不支援慣用的通訊協定，但它支援伺服器所列出的其中一個其他通訊協定，用戶端電腦就會讓伺服器知道它所支援的驗證通訊協定，並繼續進行驗證。

    -   如果用戶端電腦不支援任何列出的通訊協定，驗證交換就會失敗。

## <a name="see-also"></a>請參閱
[Windows 驗證架構](https://technet.microsoft.com/library/dn169024(v=ws.10).aspx)



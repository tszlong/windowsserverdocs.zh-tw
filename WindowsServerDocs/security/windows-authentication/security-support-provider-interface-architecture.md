---
title: 安全性支援提供者介面架構
description: Windows Server 安全性
ms.topic: article
ms.assetid: de09e099-5711-48f8-adbd-e7b8093a0336
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: da7c390427a3f0f2348d91e14d0affef905db390
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766951"
---
# <a name="security-support-provider-interface-architecture"></a>安全性支援提供者介面架構

>適用於：Windows Server (半年度管道)、Windows Server 2016

IT 專業人員的本參考主題說明 (SSPI) 架構的安全性支援提供者介面內所使用的 Windows 驗證通訊協定。

Microsoft 安全性支援提供者介面 (SSPI) 是 Windows 驗證的基礎。 需要驗證的應用程式和基礎結構服務會使用 SSPI 來提供此服務。

SSPI 是在 Windows Server 作業系統中 (GSSAPI) 的一般安全性服務 API 的實作為。 如需 GSSAPI 的詳細資訊，請參閱 IETF RFC 資料庫中的 RFC 2743 和 RFC 2744。

在 Windows 中叫用特定驗證通訊協定 (Ssp) 的預設安全性支援提供者，會以 Dll 的形式併入 SSPI 中。 下列各節將說明這些預設的 Ssp。 如果可以與 SSPI 一起運作，則可以合併其他的 Ssp。

如下圖所示，Windows 中的 SSPI 會提供一種機制，以在用戶端電腦與伺服器之間的現有通道上攜帶驗證權杖。 當兩部電腦或裝置需要經過驗證才能安全地進行通訊時，驗證的要求會路由傳送至 SSPI，以完成驗證程式，而不論目前使用的網路通訊協定為何。 SSPI 會傳回透明的二進位大型物件。 這些應用程式會在應用程式之間傳遞，此時可以傳遞給 SSPI 層。 因此，SSPI 可讓應用程式使用電腦或網路上提供的各種安全性模型，而不需要變更安全性系統的介面。

![顯示安全性支援提供者介面架構的圖表](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

下列各節說明與 SSPI 互動的預設 Ssp。 在 Windows 作業系統中，會以不同的方式使用 Ssp，在不安全的網路環境中升級安全通訊。

-   [Kerberos 安全性支援提供者](#BKMK_KerbSSP)

-   [NTLM 安全性支援提供者](#BKMK_NTLMSSP)

-   [摘要式安全性支援提供者](#BKMK_DigestSSP)

-   [安全通道安全性支援提供者](#BKMK_SchannelSSP)

-   [協商安全性支援提供者](#BKMK_NegoSSP)

-   [認證安全性支援提供者](#BKMK_CredSSP)

-   [Negotiate 延伸模組安全性支援提供者](#BKMK_NegoExtsSSP)

-   [PKU2U 安全性支援提供者](#BKMK_PKU2USSP)

本主題也包含：

[安全性支援提供者選取專案](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="kerberos-security-support-provider"></a><a name="BKMK_KerbSSP"></a>Kerberos 安全性支援提供者
此 SSP 只會使用 Microsoft 所執行的 Kerberos 第5版通訊協定。 此通訊協定是以網路工作組的 RFC 4120 和草稿修訂為基礎。 它是一種業界標準通訊協定，可搭配密碼或智慧卡用於互動式登入。 它也是 Windows 中服務的慣用驗證方法。

因為從 Windows 2000 開始，Kerberos 通訊協定是預設的驗證通訊協定，所以所有網域服務都支援 Kerberos SSP。 這些服務包括：

-   Active Directory 使用輕量型目錄存取協定 (LDAP) 的查詢

-   使用遠端程序呼叫服務的遠端伺服器或工作站管理

-   列印服務

-   用戶端-伺服器驗證

-   使用伺服器訊息區 (SMB) 通訊協定的遠端檔案存取 (也稱為 Common Internet File System 或 CIFS) 

-   分散式檔案系統管理與參考

-   Internet Information Services (IIS) 的內部網路驗證

-   網際網路通訊協定安全性 (IPsec) 的安全機構驗證

-   網域使用者和電腦的憑證要求 Active Directory 憑證服務

位置：% windir% \Windows\System32\kerberos.dll

此提供者預設包含在本主題開頭的 **適用** 物件清單中所指定的版本，以及 windows Server 2003 和 windows XP。

**Kerberos 通訊協定和 Kerberos SSP 的其他資源**

-   [Microsoft Kerberos (Windows) ](/windows/win32/secauthn/microsoft-kerberos)

-   [\[KILE \] ： Kerberos 通訊協定延伸](/openspecs/windows_protocols/ms-kile/2a32282e-dd48-4ad9-a542-609804b02cc9)

-   [\[MS-CHAP \] ： Kerberos 通訊協定延伸： Service For User 與限制委派通訊協定規格](/openspecs/windows_protocols/ms-sfu/3bff5864-8135-400e-bdd9-33b552051d94)

-   [Kerberos SSP/AP (Windows) ](/windows/win32/secauthn/kerberos-ssp-ap)

-   Windows Vista 的[Kerberos 增強功能](/previous-versions/windows/it-pro/windows-vista/cc749438(v=ws.10))

-   適用于 Windows 7 的[Kerberos 驗證變更](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560670(v=ws.10))

-   [Kerberos 驗證技術參考](/previous-versions/windows/it-pro/windows-server-2003/cc739058(v=ws.10))

### <a name="ntlm-security-support-provider"></a><a name="BKMK_NTLMSSP"></a>NTLM 安全性支援提供者
NTLM 安全性支援提供者 (NTLM SSP) 是安全性支援提供者介面所使用的二進位訊息通訊協定， (SSPI) 以允許 NTLM 挑戰-回應驗證，以及協調完整性和機密性選項。 NTLM 是在使用 SSPI 驗證的地方使用，包括伺服器訊息區或 CIFS 驗證、HTTP Negotiate 驗證 (例如，網際網路 Web 驗證) 和遠端程序呼叫服務。 NTLM SSP 包含 NTLM 和 NTLM 第2版 (NTLMv2) 驗證通訊協定。

支援的 Windows 作業系統可以使用 NTLM SSP 進行下列動作：

-   用戶端/伺服器驗證

-   列印服務

-   使用 CIFS (SMB) 的檔案存取

-   安全的遠端程序呼叫服務或 DCOM 服務

位置：% windir% \Windows\System32\msv1_0.dll

此提供者預設包含在本主題開頭的 **適用** 物件清單中所指定的版本，以及 windows Server 2003 和 windows XP。

**NTLM 通訊協定和 NTLM SSP 的其他資源**

-   [ (Windows) 的 MSV1_0 Authentication 套件 ](/windows/win32/secauthn/msv1-0-authentication-package)

-   在 Windows 7 中[進行 NTLM 驗證的變更](/previous-versions/windows/it-pro/windows-7/dd566199(v=ws.10))

-   [Microsoft NTLM (Windows)](/windows/win32/secauthn/microsoft-ntlm)

-   [稽核和限制 NTLM 使用量指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/jj865674(v=ws.10))

### <a name="digest-security-support-provider"></a><a name="BKMK_DigestSSP"></a>摘要式安全性支援提供者
摘要式驗證是一種業界標準，可用於輕量型目錄存取協定 (LDAP) 和 web 驗證。 摘要式驗證會以 MD5 雜湊或訊息摘要的方式，將認證傳送到網路上。

摘要 SSP ( # A0) 用於下列各項：

-   Internet Explorer 和 Internet Information Services (IIS) 存取

-   LDAP 查詢

位置：% windir% \Windows\System32\Digest.dll

此提供者預設包含在本主題開頭的 **適用** 物件清單中所指定的版本，以及 windows Server 2003 和 windows XP。

**摘要通訊協定和摘要 SSP 的其他資源**

-   [Microsoft Digest (Windows) 的驗證 ](/windows/win32/secauthn/microsoft-digest-ssp)

-   [\[DPSP \] ： Digest 通訊協定延伸模組](/openspecs/windows_protocols/ms-dpsp/3e44be62-2067-472a-9ef0-e937298b68fb)

### <a name="schannel-security-support-provider"></a><a name="BKMK_SchannelSSP"></a>安全通道安全性支援提供者
安全通道 (Schannel) 用於 web 伺服器驗證，例如當使用者嘗試存取安全的 web 伺服器時。

TLS 通訊協定、SSL 通訊協定、私用通訊技術 (PCT) 通訊協定，以及資料包傳輸層 (DTLS) 通訊協定是以公開金鑰加密為基礎。 Schannel 提供所有這些通訊協定。 所有的安全通道通訊協定都使用用戶端/伺服器模型。 安全通道 SSP 會使用公開金鑰憑證來驗證合作對象。 驗證合作物件時，安全通道 SSP 會依下列喜好設定順序選取通訊協定：

-   傳輸層安全性 (TLS) 1.0 版

-   傳輸層安全性 (TLS) 1.1 版

-   傳輸層安全性 (TLS) 1.2 版

-    (SSL) 2.0 版的安全通訊端層

-    (SSL) 3.0 版的安全通訊端層

-   私用通訊技術 (PCT) 

    **注意** PCT 預設為停用。

選取的通訊協定是用戶端和伺服器可支援的慣用驗證通訊協定。 例如，如果伺服器支援所有安全通道通訊協定，且用戶端僅支援 SSL 3.0 和 SSL 2.0，則驗證程式會使用 SSL 3.0。

當應用程式明確呼叫 DTLS 時，會使用此方法。 如需有關 Schannel 提供者所使用之 DTLS 和其他通訊協定的詳細資訊，請參閱 [安全通道安全性支援提供者技術參考](../tls/schannel-security-support-provider-technical-reference.md)。

位置：% windir% \Windows\System32\Schannel.dll

此提供者預設包含在本主題開頭的 **適用** 物件清單中所指定的版本，以及 windows Server 2003 和 windows XP。

> [!NOTE]
> 此提供者在 Windows Server 2008 R2 和 Windows 7 中引進了 TLS 1.2。 DTLS 是在 Windows Server 2012 和 Windows 8 的這個提供者中引進的。

**TLS 和 SSL 通訊協定和 Schannel SSP 的其他資源**

-   [安全通道 (Windows) ](/windows/win32/secauthn/secure-channel)

-   [TLS/SSL 技術參考](/previous-versions/windows/it-pro/windows-server-2003/cc784149(v=ws.10))

-   [\[TLSP \] ：傳輸層安全性 (TLS) 設定檔](/openspecs/windows_protocols/ms-tlsp/58aba05b-62b0-4cd1-b88b-dc8a24920346)

### <a name="negotiate-security-support-provider"></a><a name="BKMK_NegoSSP"></a>協商安全性支援提供者
簡單且受保護的 GSS-API 協商機制 (SPNEGO) 形成 Negotiate SSP 的基礎，whichcan 是用來協商特定的驗證通訊協定。 當應用程式呼叫 SSPI 以登入網路時，它可以指定 SSP 來處理要求。 如果應用程式指定 Negotiate SSP，則會根據客戶設定的安全性原則，來分析要求並挑選適當的提供者來處理要求。

SPNEGO 是在 RFC 2478 中指定的。

在支援的 Windows 作業系統版本中，「協商安全性支援提供者」會在 Kerberos 通訊協定與 NTLM 之間進行選取。 除非其中一個涉及驗證的系統無法使用該通訊協定，或呼叫的應用程式未提供足夠的資訊來使用 Kerberos 通訊協定，否則 Negotiate 預設會選取 Kerberos 通訊協定。

位置：% windir% \Windows\System32\lsasrv.dll

此提供者預設包含在本主題開頭的 **適用** 物件清單中所指定的版本，以及 windows Server 2003 和 windows XP。

**Negotiate SSP 的其他資源**

-   [Microsoft Negotiate (Windows) ](/windows/win32/secauthn/microsoft-negotiate)

-   [\[SPNG \] ：簡單且受保護的 GSS-API 協商機制 (SPNEGO) 擴充功能](/openspecs/windows_protocols/ms-spng/f377a379-c24f-4a0f-a3eb-0d835389e28a)

-   [\[N2HT \] ： Negotiate 和 NEGO2 HTTP 驗證通訊協定規格](/openspecs/windows_protocols/ms-n2ht/4b88aa77-4b12-4933-8740-0f32d8f4eacf)

### <a name="credential-security-support-provider"></a><a name="BKMK_CredSSP"></a>認證安全性支援提供者
 (CredSSP) 的認證安全性服務提供者，可在啟動新的終端機服務和遠端桌面服務會話時，提供單一登入 (SSO) 使用者體驗。 CredSSP 可讓應用程式根據用戶端的原則，透過伺服器端 ssp) ，從用戶端電腦將使用者的認證委派 (的用戶端電腦)  (。 CredSSP 原則是使用群組原則設定的，而且預設會關閉認證的委派。

位置：% windir% \Windows\System32\credssp.dll

此提供者預設包含在本主題開頭的 **適用** 物件清單中指定的版本中。

**認證 SSP 的其他資源**

-   [\[CSSP \] ：認證安全性支援提供者 (CredSSP) 通訊協定規格](/openspecs/windows_protocols/ms-cssp/85f57821-40bb-46aa-bfcb-ba9590b8fc30)

-   [適用于終端機服務登入的認證安全性服務提供者和 SSO](/previous-versions/windows/it-pro/windows-vista/cc749211(v=ws.10))

### <a name="negotiate-extensions-security-support-provider"></a><a name="BKMK_NegoExtsSSP"></a>Negotiate 延伸模組安全性支援提供者
 (NegoExts) 的 Negotiate 延伸模組是一種驗證套件，可針對 Microsoft 和其他軟體公司所執行的應用程式和案例，協商 NTLM 或 Kerberos 通訊協定以外的 Ssp 用法。

此 Negotiate 套件的延伸模組可允許下列案例：

-   **同盟系統內的豐富用戶端可用性。** 您可以在 SharePoint 網站上存取檔，也可以使用功能完整的 Microsoft Office 應用程式來編輯檔。

-   **Microsoft Office 服務的豐富用戶端支援。** 使用者可以登入 Microsoft Office 的服務，並使用功能完整的 Microsoft Office 應用程式。

-   **主控的 Microsoft Exchange Server 和 Outlook。** 因為 Exchange Server 裝載于 web 上，所以未建立任何網域信任。 Outlook 會使用 Windows Live 服務來驗證使用者。

-   **用戶端電腦與伺服器之間的豐富用戶端可用性。** 系統會使用作業系統的網路和驗證元件。

Windows Negotiate 套件會以與 Kerberos 和 NTLM 相同的方式來處理 NegoExts SSP。 NegoExts.dll 會在啟動時載入至本機系統授權單位 (LSA) 。 根據要求的來源收到驗證要求時，NegoExts 會在支援的 Ssp 之間進行協調。 它會收集認證和原則、對其進行加密，並將該資訊傳送至建立安全性權杖的適當 SSP。

NegoExts 支援的 Ssp 不是獨立的 Ssp，例如 Kerberos 和 NTLM。 因此，在 NegoExts SSP 內，當驗證方法因為任何原因而失敗時，將會顯示或記錄驗證失敗訊息。 無法進行重新協商或回溯驗證方法。

位置：% windir% \Windows\System32\negoexts.dll

本主題開頭的 [ **適用** 于] 清單中所指定的版本預設包含此提供者，但不包括 windows Server 2008 和 windows Vista。

### <a name="pku2u-security-support-provider"></a><a name="BKMK_PKU2USSP"></a>PKU2U 安全性支援提供者
PKU2U 通訊協定是以 Windows 7 和 Windows Server 2008 R2 中的 SSP 的形式引進和實行。 這個 SSP 會啟用對等驗證，特別是透過 Windows 7 中引進的媒體和檔案共用功能（稱為「家庭」）。 這項功能允許在非網域成員的電腦之間進行共用。

位置：% windir% \Windows\System32\pku2u.dll

本主題開頭的 [ **適用** 于] 清單中所指定的版本預設包含此提供者，但不包括 windows Server 2008 和 windows Vista。

**PKU2U 通訊協定和 PKU2U SSP 的其他資源**

-   [線上身分識別整合簡介](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560662(v=ws.10))

## <a name="security-support-provider-selection"></a><a name="BKMK_SecuritySupportProviderSelection"></a>安全性支援提供者選取專案
Windows SSPI 可以使用透過已安裝的安全性支援提供者所支援的任何通訊協定。 不過，因為並非所有的作業系統都支援與執行 Windows Server 的任何指定電腦相同的 SSP 套件，所以用戶端和伺服器必須協商使用兩者所支援的通訊協定。 Windows Server 偏好用戶端電腦和應用程式使用 Kerberos 通訊協定，這是一種強式標準的通訊協定（可能的話），但作業系統繼續允許不支援 Kerberos 通訊協定的用戶端電腦和用戶端應用程式進行驗證。

在驗證可以進行之前，兩部通訊的電腦必須同意兩者都能支援的通訊協定。 針對可透過 SSPI 使用的任何通訊協定，每部電腦都必須有適當的 SSP。 例如，若要讓用戶端電腦和伺服器使用 Kerberos 驗證通訊協定，它們都必須支援 Kerberos v5。 Windows Server 會使用函式 **EnumerateSecurityPackages** 來識別電腦支援的 ssp 以及這些 ssp 的功能。

您可以使用下列兩種方式之一來處理驗證通訊協定的選擇：

1.  [單一驗證通訊協定](#BKMK_SingleAuth)

2.  [Negotiate 選項](#BKMK_Negotiate)

### <a name="single-authentication-protocol"></a><a name="BKMK_SingleAuth"></a>單一驗證通訊協定
當伺服器上指定了單一可接受的通訊協定時，用戶端電腦必須支援指定的通訊協定，否則通訊會失敗。 當指定了單一可接受的通訊協定時，就會進行驗證交換，如下所示：

1.  用戶端電腦會要求服務的存取權。

2.  伺服器會回復要求，並指定將使用的通訊協定。

3.  用戶端電腦會檢查回復的內容，並檢查以判斷它是否支援指定的通訊協定。 如果用戶端電腦支援指定的通訊協定，則會繼續進行驗證。 如果用戶端電腦不支援此通訊協定，不論用戶端電腦是否獲授權存取資源，驗證都會失敗。

### <a name="negotiate-option"></a><a name="BKMK_Negotiate"></a>Negotiate 選項
Negotiate 選項可以用來允許用戶端和伺服器嘗試尋找可接受的通訊協定。 這是以簡單且受保護的 GSS-API 協商機制為基礎， (SPNEGO) 。 當驗證是以協商驗證通訊協定的選項開始時，SPNEGO exchange 會依照下列方式進行：

1.  用戶端電腦會要求服務的存取權。

2.  伺服器會以它所能支援的驗證通訊協定清單以及驗證挑戰或回應，根據其第一個選擇的通訊協定來回複。 例如，伺服器可能會列出 Kerberos 通訊協定和 NTLM，並傳送 Kerberos 驗證回應。

3.  用戶端電腦會檢查回復的內容，並檢查以判斷它是否支援任何指定的通訊協定。

    -   如果用戶端電腦支援慣用的通訊協定，則會繼續進行驗證。

    -   如果用戶端電腦不支援慣用的通訊協定，但它確實支援伺服器所列出的其他通訊協定，用戶端電腦會讓伺服器知道它所支援的驗證通訊協定，並繼續進行驗證。

    -   如果用戶端電腦不支援任何列出的通訊協定，驗證交換會失敗。

## <a name="additional-references"></a>其他參考資料
[Windows 驗證架構](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dn169024(v=ws.10))
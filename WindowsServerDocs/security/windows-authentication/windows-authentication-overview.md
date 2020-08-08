---
title: Windows 驗證概觀
description: Windows Server 安全性
ms.topic: article
ms.assetid: 485a0774-0785-457f-a964-0e9403c12bb1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: bb35516e16e4b7d27ab2a5b68c61fce69a0e7670
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989967"
---
# <a name="windows-authentication-overview"></a>Windows 驗證概觀

>適用於：Windows Server (半年度管道)、Windows Server 2016

這個適用於 IT 專業人員的瀏覽主題列出 Windows 驗證與登入技術的文件資源，其中包含產品評估、入門指南、程序、設計和部署指南、技術參考及命令參考。

## <a name="feature-description"></a>功能說明
驗證是確認物件、服務或個人身分識別的程序。 驗證物件時，目標是要確認該物件為正版。 當您驗證服務或個人時，目標是確認所提供的認證是否真實。

在網路內容中，驗證就是針對網路應用程式或資源提供身分識別的操作。 一般而言，身分識別是由密碼編譯作業證明，此作業只會使用使用者知道的金鑰（如同公開金鑰加密）或共用金鑰。 驗證交換的伺服器端會使用已知的密碼編譯金鑰比較簽署的資料，來驗證該驗證嘗試。

在安全的集中位置儲存密碼編譯金鑰，可以擴充和管理驗證程序。 Active Directory Domain Services 是用來儲存身分識別資訊 \( （包括使用者認證的密碼編譯金鑰）的建議和預設技術 \) 。 預設的 NTLM 與 Kerberos 實作需要 Active Directory。

驗證技術的範圍來自簡單的登入，它會根據只有使用者熟悉密碼的專案來識別使用者，使其更強大的安全性機制會使用使用者擁有的權杖、公開金鑰憑證和生物識別等專案。 在商業環境中，服務或使用者可能會在位於單一位置或跨多個位置的多種類型的伺服器上存取多個應用程式或資源。 基於上述原因，驗證必須支援其他平台與其他 Windows 作業系統的環境。

Windows 作業系統會執行一組預設的驗證通訊協定，包括 Kerberos、NTLM、傳輸層安全性 \/ 安全通訊端層 \( TLS \/ SSL \) 和摘要，作為可擴充架構的一部分。 此外，有些通訊協定會結合成驗證封裝，例如交涉與認證安全性支援提供者。 這些通訊協定與封裝可以驗證使用者、電腦以及服務；驗證程序接著可讓經過授權的使用者與服務以安全的方式存取資源。

如需 Windows 驗證的詳細資訊，包括

-   [Windows 驗證概念](windows-authentication-concepts.md)

-   [Windows 登入案例](windows-logon-scenarios.md)

-   [Windows 驗證架構](windows-authentication-architecture.md)

-   [安全性支援提供者介面架構](security-support-provider-interface-architecture.md)

-   [Windows 驗證中的認證程序](credentials-processes-in-windows-authentication.md)

-   [Windows 驗證中使用的群組原則設定](group-policy-settings-used-in-windows-authentication.md)

請參閱[Windows 驗證技術總覽](windows-authentication-technical-overview.md)。

## <a name="practical-applications"></a>實際應用
Windows 驗證是用來確認來自信任來源的資訊，是否出自個人或電腦物件，例如另一部電腦。 Windows 提供許多不同的方法來達成這個目標，如下所述。

|收件人...|功能|描述|
|----|------|--------|
|在 Active Directory 網域內驗證|Kerberos|Microsoft Windows &nbsp; Server 作業系統會針對公開金鑰驗證，執行 Kerberos 第5版的驗證通訊協定和延伸。 Kerberos 驗證用戶端會實作為安全性支援提供者 \( SSP \) ，而且可以透過安全性支援提供者介面 SSPI 來存取 \( \) 。 初始使用者驗證與 Winlogon 單一登入 \- 架構整合。 Kerberos 金鑰發佈中心 \( KDC \) 會與網域控制站上執行的其他 Windows Server 安全性服務整合。 KDC 會使用網域的 Active Directory 目錄服務資料庫作為其安全性帳戶資料庫。 預設的 Kerberos 需要 Active Directory。<p>如需其他資源，請參閱 [Kerberos 驗證概觀](../kerberos/kerberos-authentication-overview.md)。|
|在網路上安全的驗證|\/在安全通道安全性支援提供者中執行的 TLS SSL|傳輸層安全性 \( TLS \) 通訊協定版本1.0、1.1 和1.2、安全通訊端層 \( SSL \) 通訊協定、版本2.0 和3.0、資料包傳輸層安全性通訊協定版本1.0 和私人通訊傳輸 \( 百分比 \) 通訊協定（版本1.0）都是以公開金鑰密碼編譯為基礎。 安全通道 \( Schannel \) 提供者驗證通訊協定套件提供這些通訊協定。 所有安全通道通訊協定都使用用戶端與伺服器模型。<p>如需其他資源，請參閱[TLS-SSL &#40;SCHANNEL SSP&#41; 總覽](../tls/tls-ssl-schannel-ssp-overview.md)。|
|驗證 Web 服務或應用程式|整合式 Windows 驗證<p>摘要式驗證|如需其他資源，請參閱＜[整合式 Windows 驗證](/previous-versions/windows/it-pro/windows-server-2003/cc758557(v=ws.10))＞與＜[摘要式驗證](/previous-versions/windows/it-pro/windows-server-2003/cc738318(v=ws.10))＞以及＜[進階摘要式驗證](/previous-versions/windows/it-pro/windows-server-2003/cc783131(v=ws.10))＞。|
|驗證舊版應用程式|NTLM|NTLM 是一種挑戰 \- 回應樣式驗證通訊協定。除了驗證之外，NTLM 通訊協定還可選擇性地提供會話安全性，特別是透過在 NTLM 中簽署和密封函式的訊息完整性和機密性。<p>如需其他資源，請參閱 [NTLM 概觀](../kerberos/ntlm-overview.md)。|
|利用多重要素驗證|智慧卡支援<p>生物識別技術支援|智慧卡是一種防篡改 \- 且可攜的方式，可為工作提供安全性解決方案，例如用戶端驗證、登入網域、程式碼簽署，以及保護電子 \- 郵件。<p>生物識別技術仰賴測量個人不變的身體特徵，來唯一識別該位人員。 對於個人電腦與周邊設備內嵌的數百萬部指紋生物識別裝置，指紋是其中一種最常用的生物識別特徵。<p>如需其他資源，請參閱[智慧卡技術參考](/windows/security/identity-protection/smart-cards/smart-card-windows-smart-card-technical-reference)。 |
|提供本機管理、儲存以及重複使用認證的功能|認證管理<p>本機安全性授權<p>密碼|Windows 的認證管理可以確保能安全地儲存認證。 認證會 \( \) 透過應用程式或網站，在安全桌面收集，以便在每次存取資源時呈現正確的認證。<p>
|延伸舊版系統的數據機驗證保護|驗證擴充保護|這項功能會在使用整合式 Windows 驗證 IWA 來驗證網路連線時，增強認證的保護和處理 \( \) 。|

## <a name="software-requirements"></a>軟體需求
Windows 驗證是為了與舊版 Windows 作業系統相容而設計。 不過，每個版本的改進功能不一定適用於舊版。 如需詳細資訊，請參閱特定功能的文件。

## <a name="server-manager-information"></a>伺服器管理員資訊
使用伺服器管理員安裝的群組原則，可用來設定許多驗證功能。 使用伺服器管理員安裝 Windows 生物特徵辨識架構功能。 其他相依于驗證方法（例如網頁伺服器 \( IIS \) 和 Active Directory Domain Services）的伺服器角色也可以使用伺服器管理員來安裝。

## <a name="related-resources"></a>相關資源

|驗證技術|資源|
|----------------|-------|
|Windows 驗證|[Windows 驗證技術概觀](../windows-authentication/windows-authentication-technical-overview.md)<br /> 包含處理不同版本、一般驗證概念、登入案例、支援的版本架構適用設定的主題。|
|Kerberos|[Kerberos Authentication Overview](../kerberos/kerberos-authentication-overview.md)<p>[Kerberos 限制委派概觀](../kerberos/kerberos-constrained-delegation-overview.md)<p>[Kerberos 驗證技術參考](/previous-versions/windows/it-pro/windows-server-2003/cc739058(v=ws.10)) \(2003\)<p>[Kerberos 論壇](/answers/topics/windows-server-security.html)|
|TLS \/ SSL 和 DTLS \( Schannel 安全性支援提供者\)|[TLS-SSL &#40;Schannel SSP&#41; 總覽](../tls/tls-ssl-schannel-ssp-overview.md)<p>[安全通道安全性支援提供者技術參考資料](../tls/schannel-security-support-provider-technical-reference.md)|
|摘要式驗證|[摘要式驗證技術參考](/previous-versions/windows/it-pro/windows-server-2003/cc782794(v=ws.10)) \(2003\)|
|NTLM|[NTLM Overview](../kerberos/ntlm-overview.md)<br /> 包含目前和過去資源的連結|
|PKU2U|[Windows 的 PKU2U 簡介](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560634(v=ws.10))|
|智慧卡|[智慧卡技術參考](/windows/security/identity-protection/smart-cards/smart-card-windows-smart-card-technical-reference)<p>
|認證|[認證保護和管理](../credentials-protection-and-management/credentials-protection-and-management.md)<br /> 包含目前和過去資源的連結<p>[密碼總覽](../kerberos/passwords-overview.md)<br /> 包含目前和過去資源的連結|
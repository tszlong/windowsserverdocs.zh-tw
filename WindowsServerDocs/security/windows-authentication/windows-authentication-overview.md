---
title: Windows 驗證概觀
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 485a0774-0785-457f-a964-0e9403c12bb1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 2021ccf1e0015cc910f43966f9400e6bd56a230c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870969"
---
# <a name="windows-authentication-overview"></a>Windows 驗證概觀

>適用於：Windows Server （半年通道），Windows Server 2016

這個適用於 IT 專業人員的瀏覽主題列出 Windows 驗證與登入技術的文件資源，其中包含產品評估、入門指南、程序、設計和部署指南、技術參考及命令參考。

## <a name="feature-description"></a>功能描述
驗證是確認物件、服務或個人身分識別的程序。 驗證物件時，目標是要確認該物件為正版。 當您驗證服務或個人時，目標是確認所提供的認證是否真實。

在網路內容中，驗證就是針對網路應用程式或資源提供身分識別的操作。 一般而言，使用只有使用者知道-使用公開金鑰密碼編譯集或共用的金鑰的密碼編譯作業所證明身分識別。 驗證交換的伺服器端會使用已知的密碼編譯金鑰比較簽署的資料，來驗證該驗證嘗試。

在安全的集中位置儲存密碼編譯金鑰，可以擴充和管理驗證程序。 Active Directory 網域服務時的建議和預設儲存身分識別資訊的技術\(包括使用者的認證的密碼編譯金鑰\)。 預設的 NTLM 與 Kerberos 實作需要 Active Directory。

驗證技術範圍從簡單的登入，來識別使用者只有使用者知道-等更強大的安全性機制，使用使用者所擁有的項目密碼-例如權杖、 公開金鑰憑證的項目和生物識別技術。 在商業環境中，服務或使用者可能會在位於單一位置或跨多個位置的多種類型的伺服器上存取多個應用程式或資源。 基於上述原因，驗證必須支援其他平台與其他 Windows 作業系統的環境。

Windows 作業系統實作一組預設的驗證通訊協定，包括 Kerberos、 NTLM、 傳輸層安全性\/Secure Sockets Layer \(TLS\/SSL\)，及一部分的摘要，可延伸的架構。 此外，有些通訊協定會結合成驗證封裝，例如交涉與認證安全性支援提供者。 這些通訊協定與封裝可以驗證使用者、電腦以及服務；驗證程序接著可讓經過授權的使用者與服務以安全的方式存取資源。

如需 Windows 驗證的詳細資訊，包括

-   [Windows 驗證概念](windows-authentication-concepts.md)

-   [Windows 登入案例](windows-logon-scenarios.md)

-   [Windows 驗證架構](windows-authentication-architecture.md)

-   [安全性支援提供者介面架構](security-support-provider-interface-architecture.md)

-   [在 Windows 驗證的認證程序](credentials-processes-in-windows-authentication.md)

-   [Windows 驗證中使用群組原則設定](group-policy-settings-used-in-windows-authentication.md)

請參閱[Windows 驗證技術概觀](windows-authentication-technical-overview.md)。

## <a name="practical-applications"></a>實際應用
Windows 驗證是用來確認來自信任來源的資訊，是否出自個人或電腦物件，例如另一部電腦。 Windows 提供許多不同的方法來達成這個目標，如下所述。

|收件人...|功能|描述|
|----|------|--------|
|在 Active Directory 網域內驗證|Kerberos|Microsoft Windows&nbsp;伺服器作業系統，實作 Kerberos 版本 5 驗證通訊協定和延伸模組針對公開金鑰驗證。 Kerberos 驗證用戶端會實作為安全性支援提供者\(SSP\)而且可以透過安全性支援提供者介面存取\(SSPI\)。 初始使用者驗證與 Winlogon 單一登整合\-架構上。 Kerberos 金鑰發佈中心\(KDC\)與其他網域控制站上執行的 Windows Server 安全性服務整合。 KDC 使用網域的 Active Directory 目錄服務資料庫做為其安全性帳戶資料庫。 預設的 Kerberos 需要 Active Directory。<br /><br />如需其他資源，請參閱 [Kerberos 驗證概觀](../kerberos/kerberos-authentication-overview.md)。|
|在網路上安全的驗證|TLS\/SSL 安全通道安全性支援提供者中實作|傳輸層安全性\(TLS\)通訊協定版本 1.0、 1.1 及 1.2，安全通訊端層\(SSL\)通訊協定版本 2.0 和 3.0，資料包傳輸層安全性通訊協定 1.0 版，私用通訊傳輸\(PCT\)通訊協定，1.0 版，以公開金鑰密碼編譯為基礎。 安全通道\(Schannel\)提供者驗證通訊協定組合提供這些通訊協定。 所有安全通道通訊協定都使用用戶端與伺服器模型。<br /><br />如需其他資源，請參閱[TLS-SSL&#40;安全通道 SSP&#41;概觀](../tls/tls-ssl-schannel-ssp-overview.md)。|
|驗證 Web 服務或應用程式|整合式 Windows 驗證<br /><br />摘要式驗證|如需其他資源，請參閱[整合式 Windows 驗證](https://technet.microsoft.com/library/cc758557(v=WS.10).aspx)與[摘要式驗證](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx)以及[進階摘要式驗證](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx)。|
|驗證舊版應用程式|NTLM|NTLM 是一項挑戰\-回應樣式的驗證通訊協定。除了驗證，NTLM 通訊協定選擇性提供工作階段安全性，特別是訊息完整性和機密性透過簽署與密封 NTLM 中的功能。<br /><br />如需其他資源，請參閱 [NTLM 概觀](../kerberos/ntlm-overview.md)。|
|利用多重要素驗證|智慧卡支援<br /><br />生物識別技術支援|智慧卡是防竄改\-竄改的可攜式工具，來提供安全性解決方案的工作，例如用戶端驗證、 登入網域，程式碼簽署及保護電子\-郵件。<br /><br />生物識別技術仰賴測量個人不變的身體特徵，來唯一識別該位人員。 對於個人電腦與周邊設備內嵌的數百萬部指紋生物識別裝置，指紋是其中一種最常用的生物識別特徵。<br /><br />如需其他資源，請參閱[智慧卡技術參考](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)。 |
|提供本機管理、儲存以及重複使用認證的功能|認證管理<br /><br />本機安全性授權<br /><br />密碼|Windows 的認證管理可以確保能安全地儲存認證。 認證會收集在安全桌面上\(本機或網域存取\)、 透過應用程式或網站，以便每次存取資源時，會顯示正確的認證。<br /><br />
|延伸舊版系統的數據機驗證保護|驗證延伸保護|這項功能增強的保護與處理的認證驗證網路連線使用整合式 Windows 驗證時\(IWA\)。|

## <a name="software-requirements"></a>軟體需求
Windows 驗證是為了與舊版 Windows 作業系統相容而設計。 不過，每個版本的改進功能不一定適用於舊版。 如需詳細資訊，請參閱特定功能的文件。

## <a name="server-manager-information"></a>伺服器管理員資訊
使用伺服器管理員安裝的群組原則，可用來設定許多驗證功能。 使用伺服器管理員安裝 Windows 生物特徵辨識架構功能。 其他依存於驗證方法，例如 Web 伺服器的伺服器角色\(IIS\)和 Active Directory 網域服務，也可以安裝使用伺服器管理員。

## <a name="related-resources"></a>相關資源

|驗證技術|資源|
|----------------|-------|
|Windows 驗證|[Windows 驗證技術概觀](../windows-authentication/windows-authentication-technical-overview.md)<br />包含處理不同版本、 一般驗證概念、 登入案例、 支援的版本架構適用設定的主題。|
|Kerberos|[Kerberos 驗證概觀](../kerberos/kerberos-authentication-overview.md)<br /><br />[Kerberos 限制委派概觀](../kerberos/kerberos-constrained-delegation-overview.md)<br /><br />[Kerberos 驗證技術參考](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003年\)<br /><br />[Kerberos 存續指南](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx) \(TechNet Wiki\)|
|TLS\/SSL 和 DTLS\(安全通道安全性支援提供者\)|[TLS-SSL&#40;安全通道 SSP&#41;概觀](../tls/tls-ssl-schannel-ssp-overview.md)<br /><br />[安全通道安全性支援提供者技術參考](../tls/schannel-security-support-provider-technical-reference.md)|
|摘要式驗證|[摘要式驗證技術參考](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003年\)|
|NTLM|[NTLM 概觀](../kerberos/ntlm-overview.md)<br />包含目前和過去資源的連結|
|PKU2U|[Windows 的 PKU2U 簡介](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|智慧卡|[智慧卡技術參考](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<br /><br />
|認證|[認證保護和管理](../credentials-protection-and-management/credentials-protection-and-management.md)<br />包含目前和過去資源的連結<br /><br />[密碼概觀](../kerberos/passwords-overview.md)<br />包含目前和過去資源的連結|



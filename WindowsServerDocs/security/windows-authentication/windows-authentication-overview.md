---
title: Windows 驗證概觀
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 262a510e0b8484f3ee5cc28726f10857f92cbfd4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403287"
---
# <a name="windows-authentication-overview"></a>Windows 驗證概觀

>適用於：Windows Server (半年度管道)、Windows Server 2016

這個適用於 IT 專業人員的瀏覽主題列出 Windows 驗證與登入技術的文件資源，其中包含產品評估、入門指南、程序、設計和部署指南、技術參考及命令參考。

## <a name="feature-description"></a>功能描述
驗證是確認物件、服務或個人身分識別的程序。 驗證物件時，目標是要確認該物件為正版。 當您驗證服務或個人時，目標是確認所提供的認證是否真實。

在網路內容中，驗證就是針對網路應用程式或資源提供身分識別的操作。 一般而言，身分識別是由密碼編譯作業證明，此作業只會使用使用者知道的金鑰（如同公開金鑰加密）或共用金鑰。 驗證交換的伺服器端會使用已知的密碼編譯金鑰比較簽署的資料，來驗證該驗證嘗試。

在安全的集中位置儲存密碼編譯金鑰，可以擴充和管理驗證程序。 Active Directory Domain Services 是用來儲存身分識別資訊的建議和預設技術 \(including 使用者認證 @ no__t-1 的密碼編譯金鑰。 預設的 NTLM 與 Kerberos 實作需要 Active Directory。

驗證技術的範圍來自簡單的登入，它會根據只有使用者熟悉密碼的專案來識別使用者，提供更強大的安全性機制，使用使用者擁有的權杖、公開金鑰憑證，以及指標. 在商業環境中，服務或使用者可能會在位於單一位置或跨多個位置的多種類型的伺服器上存取多個應用程式或資源。 基於上述原因，驗證必須支援其他平台與其他 Windows 作業系統的環境。

Windows 作業系統會執行一組預設的驗證通訊協定，包括 Kerberos、NTLM、傳輸層安全性 @ no__t-0Secure 通訊端層 \(TLS @ no__t-2SSL @ no__t-3 和 Digest，做為可擴充架構的一部分。 此外，有些通訊協定會結合成驗證封裝，例如交涉與認證安全性支援提供者。 這些通訊協定與封裝可以驗證使用者、電腦以及服務；驗證程序接著可讓經過授權的使用者與服務以安全的方式存取資源。

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
|在 Active Directory 網域內驗證|Kerberos|Microsoft Windows @ no__t 0Server 作業系統會針對公開金鑰驗證執行 Kerberos 版本5驗證通訊協定和延伸。 Kerberos 驗證用戶端會實作為安全性支援提供者 \(SSP @ no__t-1，而且可以透過安全性支援提供者介面 \(SSPI @ no__t-3 來存取。 初始使用者驗證與 Winlogon 單一登入 @ no__t-0on 架構整合。 Kerberos 金鑰發佈中心 \(KDC @ no__t-1 已與網域控制站上執行的其他 Windows Server 安全性服務整合。 KDC 會使用網域的 Active Directory 目錄服務資料庫作為其安全性帳戶資料庫。 預設的 Kerberos 需要 Active Directory。<br /><br />如需其他資源，請參閱 [Kerberos 驗證概觀](../kerberos/kerberos-authentication-overview.md)。|
|在網路上安全的驗證|TLS @ no__t-在 Schannel 安全性支援提供者中執行0SSL|傳輸層安全性 \(TLS @ no__t-1 通訊協定版本1.0、1.1 和1.2，安全通訊端層 \(SSL @ no__t-3 通訊協定，版本2.0 和3.0，資料包傳輸層安全性通訊協定版本1.0 和私人通訊傳輸@no__t 4PCT @ no__t-5 通訊協定版本1.0，是以公開金鑰密碼編譯為基礎。 安全通道 \(Schannel @ no__t-1 提供者驗證通訊協定套件提供這些通訊協定。 所有安全通道通訊協定都使用用戶端與伺服器模型。<br /><br />如需其他資源，請參閱[TLS &#40;-SSL&#41; Schannel 的 SSP 總覽](../tls/tls-ssl-schannel-ssp-overview.md)。|
|驗證 Web 服務或應用程式|整合式 Windows 驗證<br /><br />摘要式驗證|如需其他資源，請參閱[整合式 Windows 驗證](https://technet.microsoft.com/library/cc758557(v=WS.10).aspx)與[摘要式驗證](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx)以及[進階摘要式驗證](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx)。|
|驗證舊版應用程式|NTLM|NTLM 是一項挑戰 @ no__t-0response style 驗證通訊協定。除了驗證之外，NTLM 通訊協定還可選擇性地提供會話安全性，特別是透過在 NTLM 中簽署和密封函式的訊息完整性和機密性。<br /><br />如需其他資源，請參閱 [NTLM 概觀](../kerberos/ntlm-overview.md)。|
|利用多重要素驗證|智慧卡支援<br /><br />生物識別技術支援|智慧卡是一種篡改 @ no__t-0resistant 且可移植的方式，可為工作提供安全性解決方案，例如用戶端驗證、登入網域、程式碼簽署，以及保護 e @ no__t-1mail。<br /><br />生物識別技術仰賴測量個人不變的身體特徵，來唯一識別該位人員。 對於個人電腦與周邊設備內嵌的數百萬部指紋生物識別裝置，指紋是其中一種最常用的生物識別特徵。<br /><br />如需其他資源，請參閱[智慧卡技術參考](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)。 |
|提供本機管理、儲存以及重複使用認證的功能|認證管理<br /><br />本機安全性授權<br /><br />密碼|Windows 的認證管理可以確保能安全地儲存認證。 認證會透過應用程式或透過網站，在安全的桌面上收集 @no__t 0for 本機或網域存取 @ no__t-1，以便在每次存取資源時呈現正確的認證。<br /><br />
|延伸舊版系統的數據機驗證保護|驗證延伸保護|這項功能會在使用整合式 Windows 驗證（\(IWA @ no__t-1）驗證網路連線時，增強認證的保護和處理。|

## <a name="software-requirements"></a>軟體需求
Windows 驗證是為了與舊版 Windows 作業系統相容而設計。 不過，每個版本的改進功能不一定適用於舊版。 如需詳細資訊，請參閱特定功能的文件。

## <a name="server-manager-information"></a>伺服器管理員資訊
使用伺服器管理員安裝的群組原則，可用來設定許多驗證功能。 使用伺服器管理員安裝 Windows 生物特徵辨識架構功能。 其他相依于驗證方法的伺服器角色，例如 Web 服務器 \(IIS @ no__t-1 和 Active Directory Domain Services，也可以使用伺服器管理員來安裝。

## <a name="related-resources"></a>相關資源

|驗證技術|資源|
|----------------|-------|
|Windows 驗證|[Windows 驗證技術概觀](../windows-authentication/windows-authentication-technical-overview.md)<br />包含一些主題，可解決版本、一般驗證概念、登入案例、支援版本的架構，以及適用的設定之間的差異。|
|Kerberos|[Kerberos 驗證總覽](../kerberos/kerberos-authentication-overview.md)<br /><br />[Kerberos 限制委派概觀](../kerberos/kerberos-constrained-delegation-overview.md)<br /><br />[Kerberos 驗證技術參考](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003 @ no__t-2<br /><br />[Kerberos 生存指南](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx)\(TechNet Wiki @ no__t-2|
|TLS @ no__t-0SSL 和 DTLS \(Schannel 安全性支援提供者 @ no__t-2|[TLS-SSL &#40;Schannel SSP&#41;總覽](../tls/tls-ssl-schannel-ssp-overview.md)<br /><br />[安全通道安全性支援提供者技術參考資料](../tls/schannel-security-support-provider-technical-reference.md)|
|摘要式驗證|[摘要式驗證技術參考](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003 @ no__t-2|
|NTLM|[NTLM 總覽](../kerberos/ntlm-overview.md)<br />包含目前和過去資源的連結|
|PKU2U|[Windows 中的 PKU2U 簡介](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|智慧卡|[智慧卡技術參考](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<br /><br />
|認證|[認證保護和管理](../credentials-protection-and-management/credentials-protection-and-management.md)<br />包含目前和過去資源的連結<br /><br />[密碼總覽](../kerberos/passwords-overview.md)<br />包含目前和過去資源的連結|



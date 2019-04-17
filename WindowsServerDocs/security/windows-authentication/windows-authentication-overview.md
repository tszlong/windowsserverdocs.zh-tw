---
title: "Windows 驗證的概觀"
description: "Windows Server 安全性"
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
ms.openlocfilehash: c2ec55ed6b09628d1d80a24be766259980e84d6e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="windows-authentication-overview"></a>Windows 驗證的概觀

>適用於：Windows Server（以每年次管道）、Windows Server 2016

適用於 IT 專業人員本瀏覽主題列出文件，包括 product 評估版，取得入門的指南、程序、設計和部署指南、技術的資訊尋找參考資料，以及命令參照 Windows 驗證並登入技術資源。

## <a name="feature-description"></a>描述的功能
驗證是以驗證身分物件、服務或連絡人的處理程序。 當您進行驗證物件時，目標是驗證物件是正版軟體。 當您進行驗證服務或連絡人時，目標是驗證使用者出示的真確。

網路功能的操作，以驗證是網路的應用程式或資源證明身分的行為。 一般而言，身分是證明使用只使用者知道-與公用加密-或共用的按鍵任一個按鍵的密碼編譯作業。 驗證換貨的伺服器端比較簽署的資料與已知驗證嘗試密碼編譯金鑰。

延展性，而且可維護密碼編譯金鑰儲存在安全的中央位置可驗證程序。 Active Directory Domain Services 是建議與儲存的身分的資訊預設技術 \（包括密碼編譯金鑰的使用者的 credentials\）。 Active Directory 是必要的預設 NTLM 和 Kerberos 實作。

驗證技術範圍從簡單登入，將根據項目僅使用者知道-密碼，例如使用的使用者-權杖、公開金鑰憑證，並生物等項目越安全機制使用者辨識。 在企業環境中，服務或使用者可能會存取多個應用程式或許多類型的在單一位置或跨多個位置的伺服器上的資源。 基於這些原因，驗證必須支援其他平台和其他 Windows 作業系統的環境。

Windows 作業系統實作驗證通訊協定，包括 Kerberos，NTLM，預設設定傳輸層 Security\ 日安全通訊端層 \(TLS\/SSL\)，以及摘要、延伸架構的一部分。 此外，部分通訊協定的結合成交涉和認證安全性支援提供者驗證套件。 這些通訊協定與套件讓驗證使用者、電腦及服務。驗證程序，就能授權的使用者與服務存取資源在安全的方式。

如需有關包括 Windows 驗證

-   [Windows 驗證概念](windows-authentication-concepts.md)

-   [Windows 登入案例](windows-logon-scenarios.md)

-   [Windows 驗證架構](windows-authentication-architecture.md)

-   [安全性支援提供者介面架構](security-support-provider-interface-architecture.md)

-   [在 Windows 驗證認證處理程序](credentials-processes-in-windows-authentication.md)

-   [Windows 驗證中使用群組原則設定](group-policy-settings-used-in-windows-authentication.md)

查看[Windows 驗證技術概觀](windows-authentication-technical-overview.md)。

## <a name="practical-applications"></a>實用的應用程式
若要確認是否的人員或電腦的物件，例如另一部電腦的資訊，都來自信任的來源，使用 Windows 驗證。 Windows 提供許多不同的方法達成這個目標，如下所述。

|若要...|功能|描述|
|----|------|--------|
|在 Active Directory domain 驗證|Kerberos|Microsoft Windows&nbsp;伺服器作業系統實作 Kerberos 5 版本驗證通訊協定與公開金鑰驗證擴充功能。 做為安全性支援提供者實作 Kerberos 驗證 client \(SSP\) 及可支援提供者介面安全性 \(SSPI\) 透過存取。 初次使用者驗證整合 Winlogon 單一 sign\ 上架構。 與其他 Windows Server 安全性服務執行的網域控制站 Kerberos 金鑰 Distribution 中心 \(KDC\) 整合。 \ [KDC 使用網域的 Active Directory directory 服務資料庫做為其安全性 account 資料庫。 Active Directory 是必要的預設 Kerberos 實作。<br /><br />資源，請查看[Kerberos 驗證的概觀](../kerberos/kerberos-authentication-overview.md)。|
|安全網路上的驗證|TLS\ 日 SSL 實作 Schannel 安全性支援提供者|Tls \(TLS\) 通訊協定 1.0，1.1、1.2 安全通訊端層 \(SSL\) 通訊協定，2.0 及 3.0 資料流 Tls 通訊協定第版版本 1.0，以及 \(PCT\) 私人通訊傳輸通訊協定 1.0，依據公開加密。 安全通道 \(Schannel\) 提供者驗證通訊協定提供這些通訊協定。 所有 Schannel 通訊協定都使用 client 和伺服器模型。<br /><br />資源，請查看[TLS-SSL 和 #40;Schannel SSP 和 #41;概觀](../tls/tls-ssl-schannel-ssp-overview.md)。|
|驗證的應用程式或 web 服務|整合式的 Windows 驗證<br /><br />摘要驗證|適用於額外的資源，查看 [整合式 Windows 驗證] (https://technet.microsoft.com/library/cc758557(v=WS.10.aspx and [Digest Authentication](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx), and [Advanced Digest Authentication](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx).|
|驗證舊版應用程式|NTLM|NTLM 是驗證 challenge\-回應樣式驗證 protocol.In 新增，對工作階段安全性-專門訊息完整性和機密性透過登入和密封 NTLM 中的功能也提供 NTLM 通訊協定。<br /><br />資源，請查看[NTLM 概觀](../kerberos/ntlm-overview.md)。|
|利用要素|智慧卡的支援<br /><br />生物特徵辨識支援|智慧卡是 tamper\ 竄改和可移植來提供安全性方案 client 驗證，登入網域，例如工作的程式碼簽章和保護 e\ 郵件。<br /><br />測量的唯一找出該人員將某位連絡人不會變更實體特性依賴生物。 指紋的其中一個最常使用的生物特徵辨識特性，數百萬指紋的個人電腦和周邊設備 embedded 的生物特徵辨識裝置。<br /><br />資源，請查看[智慧卡技術參考](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)。 |
|提供本機管理、儲存及重複使用的憑證|認證管理<br /><br />本機安全性授權<br /><br />密碼|Windows 認證管理確保安全地儲存認證。 在安全桌面上會收集認證 \（適用於本機或網域 access\)，透過應用程式或的網站，以便在正確的憑證會顯示每次存取資源。<br /><br />
|延長現代化驗證傳統系統的保護|驗證延伸的保護|這項功能美化保護與認證處理時使用的整合式 Windows 驗證 \(IWA\) 驗證網路連接。|

## <a name="software-requirements"></a>軟體需求
Windows 驗證被設計為先前的 Windows 作業系統版本與相容。 不過，並非一定適用於舊版的每個版本的改進。 如需詳細資訊的特定功能的相關文件，請參考。

## <a name="server-manager-information"></a>伺服器管理員資訊
您可以使用群組原則，可以使用伺服器管理員安裝設定許多驗證功能。 使用伺服器管理員安裝 Windows 的生物特徵辨識架構功能。 其他伺服器角色依賴驗證方法、網頁伺服器 \(IIS\) 和 Active Directory Domain Services，這也可以使用伺服器管理員會安裝。

## <a name="related-resources"></a>相關的資源

|驗證技術|資源|
|----------------|-------|
|Windows 驗證|[Windows 驗證技術概觀](../windows-authentication/windows-authentication-technical-overview.md)<br />包含主題位址設定不同的版本，一般驗證概念、登入案例，針對支援的版本，並設定適用的架構。|
|Kerberos|[Kerberos 驗證的概觀](../kerberos/kerberos-authentication-overview.md)<br /><br />[Kerberos 限制委派概觀](../kerberos/kerberos-constrained-delegation-overview.md)<br /><br />[Kerberos 驗證技術參考](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003\)<br /><br />[Kerberos 存續指南](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx)\(TechNet Wiki\)|
|TLS\ 日 SSL 和 DTLS \ (Schannel 安全性支援 provider\)|[TLS-SSL 和 #40;Schannel SSP 和 #41;概觀](../tls/tls-ssl-schannel-ssp-overview.md)<br /><br />[Schannel 安全性支援提供者技術參考](../tls/schannel-security-support-provider-technical-reference.md)|
|摘要驗證|[摘要驗證技術參考](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003\)|
|NTLM|[NTLM 概觀](../kerberos/ntlm-overview.md)<br />包含目前與過去資源連結|
|PKU2U|[在 Windows 中可能造成 PKU2U](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|智慧卡|[智慧卡技術參考](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<br /><br />
|認證|[認證保護與管理](../credentials-protection-and-management/credentials-protection-and-management.md)<br />包含目前與過去資源連結<br /><br />[密碼概觀](../kerberos/passwords-overview.md)<br />包含目前與過去資源連結|



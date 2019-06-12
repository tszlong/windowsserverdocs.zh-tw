---
title: Alwayson VPN 技術概觀
description: '此頁面 」 提供以一律開啟 」 VPN 技術的詳細文件的連結的簡短概觀。 '
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 65a575b24ea3c70ad7eedd95fe287d955ccaeea6
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749680"
---
# <a name="always-on-vpn-technology-overview"></a>Alwayson VPN 技術概觀

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows 10

- [**前一個：** 了解一律開啟 」 VPN 增強功能](always-on-vpn-enhancements.md)
- [**下一步:** 深入了解一律開啟 」 VPN 的進階功能](deploy/always-on-vpn-adv-options.md)

針對此部署，您必須安裝新的遠端存取伺服器是執行 Windows Server 2016 中，以及修改某些現有的基礎結構部署。

下圖顯示才能部署 「 一律開啟 」 VPN 基礎結構。

![Alwayson VPN 基礎結構](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

在此圖中所述的連線程序包含下列步驟：

1. Windows 10 VPN 用戶端使用公用 DNS 伺服器，VPN 閘道的 IP 位址執行名稱解析查詢。

2. 使用 DNS 所傳回的 IP 位址，VPN 用戶端會傳送至 VPN 閘道連線要求。

3. VPN 閘道也會設定為遠端驗證撥號使用者服務 (RADIUS) 用戶端;VPN RADIUS 用戶端會傳送到組織/公司 NPS 伺服器的連線要求處理連線要求。

4. NPS 伺服器處理連線要求，包括執行授權和驗證，並決定是否要允許或拒絕連線要求。

5. NPS 伺服器會將轉送至 VPN 閘道的存取接受或拒絕存取的回應。

6. 起始連接或終止根據從 NPS 伺服器的 VPN 伺服器收到的回應。

如需有關在上圖中所述的每個基礎結構元件的詳細資訊，請參閱下列各節。

>[!NOTE]
>如果您已經有一些在網路上部署這些技術，您可以使用此部署指南中的指示用於此部署中執行的技術的其他設定。

## <a name="domain-name-system-dns"></a>網域名稱系統 (DNS)

內部和外部網域名稱系統 (DNS) 區域是必要的它會假設內部區域是外部的區域 （例如，corp.contoso.com 和 contoso.com） 的委派子網域。

深入了解[網域名稱系統 (DNS)](../../../../networking/dns/dns-top.md)或是[核心網路指南](../../../../networking/core-network-guide/core-network-guide.md)。

>[!NOTE]
>其他 DNS 設計，例如拆分式 DNS （使用相同的網域名稱內部和外部個別 DNS 區域中），或不相關的內部和外部網域 （例如 contoso.local 和 contoso.com） 也是可行的。 如需部署拆分式 DNS 的詳細資訊，請參閱 < [Split-Brain DNS 部署的使用 DNS 原則](../../../../networking/dns/deploy/split-brain-DNS-deployment.md)。

## <a name="firewalls"></a>防火牆

請確定您的防火牆允許流量所需的 VPN 和 RADIUS 通訊，才能正確運作。

如需詳細資訊，請參閱 <<c0> [ 設定用於 RADIUS 流量的防火牆](../../../../networking/technologies/nps/nps-firewalls-configure.md)。

## <a name="remote-access-as-a-ras-gateway-vpn-server"></a>RAS 閘道的 VPN 伺服器的遠端存取

在 Windows Server 2016 中，遠端存取伺服器角色可執行以及路由器和遠端存取伺服器;因此，它支援各式各樣的功能。 本部署指南中，您需要這些功能一小部分： 支援 IKEv2 VPN 連線和區域網路路由。

IKEv2 是 VPN 通道通訊協定的註解 7296 網際網路工程的任務推動小組要求中所述。 IKEv2 的主要優點是它可容許在基礎的網路連線中斷。 比方說，如果連線暫時中斷，或使用者將用戶端電腦從一個網路移至另一個，IKEv2 會自動還原 VPN 連線的網路連線重新建立時，不需要使用者介入。

藉由使用 RAS 閘道，您可以部署至使用者提供遠端存取您組織的網路和資源的 VPN 連線。 只要遠端電腦連線到網際網路，則部署一律開啟 」 VPN 會維護用戶端與您的組織網路之間的持續連線。 透過 RAS 閘道，您也可以建立不同位置的兩部伺服器之間的站對站 VPN 連線這類之間主要辦公室及分公司辦公室，及使用網路位址轉譯 (NAT)，以便在網路內的使用者可以存取外部例如，網際網路的資源。 此外，RAS 閘道支援邊界閘道通訊協定 (BGP)，可提供動態路由的服務，當遠端辦公室位置也有邊緣閘道支援 BGP。

您可以使用 Windows PowerShell 命令和遠端存取 Microsoft Management Console (MMC) 來管理遠端存取服務 (RAS) 閘道。

## <a name="network-policy-server-nps"></a>網路原則伺服器 (NPS)

NPS 可讓您建立和強制執行整個組織的網路連線要求驗證和授權的存取原則。 當您使用 NPS 做為遠端驗證撥號使用者服務 (RADIUS) 伺服器時，您會設定為 NPS 中的 RADIUS 用戶端的網路存取伺服器，例如 VPN 伺服器。

您也可以設定 NPS 用來授權連線要求的網路原則，並且可以設定 RADIUS 帳戶處理，讓 NPS 將計量資訊記錄到本機硬碟上或 Microsoft SQL Server 資料庫中的記錄檔。

如需詳細資訊，請參閱 <<c0> [ 網路原則伺服器 (NPS)](../../../../networking/technologies/nps/nps-top.md)。

## <a name="active-directory-certificate-services"></a>Active Directory 憑證服務

正在執行 Active Directory 憑證服務的憑證授權單位憑證授權單位 (CA) 伺服器。 VPN 設定需要 Active Directory 型公開金鑰基礎結構 (PKI)。

組織可以使用 AD CS 來加強安全性繫結至對應的公開金鑰的個人、 裝置或服務的身分識別。 AD CS 也包含可讓您管理各種彈性環境中的憑證註冊與撤銷的功能。 如需詳細資訊，請參閱 < [Active Directory 憑證服務概觀](https://technet.microsoft.com/library/hh831740.aspx)並[公用金鑰基礎結構設計指引](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx)。

在部署完成後，您會在 CA 上設定下列憑證範本。

- 使用者驗證憑證範本

- VPN 伺服器驗證憑證範本

- NPS 伺服器驗證憑證範本

### <a name="certificate-templates"></a>憑證範本

憑證範本可讓您預先設定的發行憑證的所選的工作，可大幅簡化管理憑證授權單位 (CA) 的工作。 [憑證範本] MMC 嵌入式管理單元可讓您執行下列工作。

- 檢視每個憑證範本的內容。

- 複製並修改憑證範本。

- 控制哪些使用者和電腦可以讀取範本，並註冊憑證。

- 執行與憑證範本相關的其他管理工作。

憑證範本是企業憑證授權單位 (CA) 中不可或缺的一部分。 也就是為了讓環境，也就是一組規則和格式的憑證註冊、 使用及管理憑證原則的重要項目。

如需詳細資訊，請參閱 <<c0> [ 憑證範本](https://technet.microsoft.com/library/cc730705.aspx)。

### <a name="digital-server-certificates"></a>數位伺服器憑證

本部署指南提供使用 Active Directory 憑證服務 (AD CS) 註冊和自動註冊憑證給遠端存取和 NPS 基礎結構伺服器的指示。 AD CS 可以讓您建置公開金鑰基礎結構 (PKI)，並為您的組織提供公開金鑰密碼編譯、 數位憑證以及數位簽章功能。

當您使用您網路上電腦之間進行驗證的數位伺服器憑證時，憑證提供：

1. 透過加密的機密性。

2. 透過數位簽章的完整性。

3. 由關聯的電腦、 使用者或裝置的帳戶在電腦網路上的憑證金鑰的驗證。

如需詳細資訊，請參閱[AD CS 逐步解說指南：兩層 PKI 階層部署](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)。

## <a name="active-directory-domain-services-ad-ds"></a>Active Directory 網域服務 (AD DS)

AD DS 提供一個分散式資料庫，用來儲存和管理網路資源的相關資訊，以及啟用目錄之應用程式的應用程式特定資料。 系統管理員可以使用 AD DS 將網路項目 (像是使用者、電腦以及其他裝置) 組織成階層式的內含項目結構。 階層式內含項目結構包含 Active Directory 樹系、樹系中的網域以及每個網域中的組織單位 (OU)。 執行 AD DS 的伺服器稱為網域控制站。

AD DS 包含使用者帳戶、 電腦帳戶和所需的受保護的可延伸驗證通訊協定 (PEAP)，來驗證使用者認證，並評估 VPN 連線要求授權的帳戶屬性。 如需部署 AD DS 的相關資訊，請參閱 Windows Server 2016[核心網路指南](../../../../networking/core-network-guide/Core-Network-Guide.md)。

在此部署中的步驟完成後，您將網域控制站上設定下列項目。

- 啟用群組原則中的電腦和使用者的憑證自動註冊

- 建立 VPN 使用者群組

- 建立 VPN 伺服器群組

- 建立 NPS 伺服器群組

### <a name="active-directory-users-and-computers"></a>Active Directory 使用者和電腦

Active Directory 使用者和電腦是元件的 AD DS 中包含代表實體的實體，例如電腦、 個人、 群組或安全性群組帳戶。 安全性群組是系統管理員可以為單一單位管理的使用者或電腦帳戶的集合。 屬於特定群組的使用者和電腦帳戶會稱為群組成員。

在 Active Directory 使用者和電腦的使用者帳戶具有撥入內容，NPS 會評估授權程序-期間，除非**網路存取權限**的使用者帳戶的屬性設定為**控制項透過 NPS 網路原則存取**。 這是所有的使用者帳戶的預設設定。 不過，在某些情況下，這項設定可能會有不同的設定會禁止使用者使用 VPN 連線。 若要防止這種可能性，您可以設定 NPS 伺服器，以略過使用者帳戶撥入內容。

如需詳細資訊，請參閱 <<c0> [ 略過使用者帳戶撥入內容設定 NPS](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties)。

### <a name="group-policy-management"></a>群組原則管理

群組原則管理可讓目錄基礎變更和組態管理的使用者和電腦的設定，包括安全性和使用者的資訊。 您可以使用群組原則來定義群組的使用者和電腦組態。

使用群組原則，您可以指定登錄項目、 安全性、 軟體安裝、 指令碼、 資料夾重新導向、 遠端安裝服務，以及 Internet Explorer 維護設定。 您所建立的群組原則設定會包含在群組原則物件 (GPO)。 由關聯所選取的 Active Directory 系統容器中的 GPO — 網站、 網域及 Ou — 您可以將 GPO 的設定套用至使用者和電腦的 Active Directory 容器中。 若要管理整個企業的群組原則物件，您可以使用群組原則管理編輯器 Microsoft Management Console (MMC)。

## <a name="windows-10-vpn-clients"></a>Windows 10 VPN 用戶端

除了伺服器元件，請確定您設定要使用 VPN 用戶端電腦正在執行 Windows 10 年度更新版 （1607年版）。 Windows 10 VPN 用戶端必須已加入網域的 Active Directory 網域。


Windows 10 VPN 用戶端設定，並提供許多選項。 為了更清楚說明此案例使用的特定功能，表 1 會識別此部署會參考的 VPN 功能的類別和特定的組態。 您將使用此部署在稍後討論的 VPNv2 組態服務提供者 (CSP) 設定個別設定這些功能的。 

表 1. VPN 功能和此部署中所述的組態

| VPN 功能     |     部署案例的組態         |
|-----------------|-----------------------------------------------|
| 連線類型 |                 Native IKEv2                  |
|     路由     |                分割通道                |
| 名稱解析 |  網域名稱資訊清單和 DNS 尾碼  |
|   觸發    |    Always On 且受信任網路偵測    |
| 驗證  | PEAP-TLS 使用 TPM 所保護的使用者憑證 |

>[!NOTE]
>PEAP-TLS 與 TPM 分別為 「 受保護的可延伸驗證通訊協定與傳輸層安全性 」 和 「 受信任的平台模組 」。

### <a name="vpnv2-csp-nodes"></a>VPNv2 CSP 節點

在此部署中，您可以使用 ProfileXML VPNv2 CSP 節點來建立 VPN 設定檔傳遞給 Windows 10 用戶端電腦。 組態服務提供者 (Csp) 是公開 Windows 用戶端; 內的各種管理功能的介面就概念而言，Csp 的運作類似於群組原則的運作方式。 每一個 CSP 都代表個別設定的組態節點。 也像是群組原則設定，您可以繫結 CSP 設定登錄機碼、 檔案、 權限，等等。 類似於您如何使用群組原則管理編輯器來設定群組原則物件 (Gpo)，您設定的 CSP 節點使用 Microsoft Intune 等行動裝置管理 (MDM) 解決方案。 例如 Intune MDM 產品提供方便使用的組態選項，可在作業系統中設定的 CSP。

![CSP 設定的行動裝置管理](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

不過，您無法設定某些 CSP 節點，直接透過像是 Intune 管理主控台的使用者介面 (UI)。 在這些情況下，您必須設定開放行動聯盟統一資源識別項 (OMA-URI) 設定以手動方式。 您可以使用 OMA 裝置管理通訊協定 (OMA-DM)，支援最新的 Apple、 Android 和 Windows 裝置的通用裝置管理規格，以設定 OMA Uri。 只要它們符合 OMA-DM 規格，所有 MDM 產品應該相同的方式，與這些作業系統上都互動。

Windows 10 提供許多 Csp，但此部署，著重於使用 VPNv2 CSP 設定 VPN 用戶端。 VPNv2 CSP 會允許在 Windows 10 中的每個 VPN 設定檔設定的設定，透過唯一的 CSP 節點。 也包含在 VPNv2 CSP 是節點，稱為*ProfileXML*，可讓您設定所有設定，在一個節點而非個別。 如需 ProfileXML 的詳細資訊，請參閱稍後在此部署中的 < ProfileXML 概觀 > 一節。 如需 VPNv2 CSP 中的每個節點的詳細資訊，請參閱[VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp)。

## <a name="next-steps"></a>後續步驟

- [深入了解一些進階的一律開啟 」 VPN 功能](deploy/always-on-vpn-adv-options.md)

- [開始規劃您的一律開啟 」 VPN 部署](deploy/always-on-vpn-deploy-deployment.md)

## <a name="related-topics"></a>相關主題

- [Microsoft Azure 虛擬機器的 Microsoft 伺服器軟體支援](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines):這篇文章討論在 Microsoft Azure 虛擬機器環境 （基礎結構做為服務） 中執行 Microsoft 伺服器軟體的支援原則。

- [遠端存取](../../Remote-Access.md):本主題提供 Windows Server 2016 中的遠端存取伺服器角色的概觀。

- [Windows 10 VPN 技術指南](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide):本指南會引導您完成將您的 VPN 解決方案的企業中的 Windows 10 用戶端以及如何設定您的部署決策。 本指南參考 VPNv2 組態服務提供者 (CSP)，並提供行動裝置管理 (MDM) 設定的指示適用於 Windows 10 中使用 Microsoft Intune 和 VPN 設定檔範本。

- [核心網路指南](../../../../networking/core-network-guide/Core-Network-Guide.md):本指南提供有關如何規劃和部署全功能網路與新樹系中的新 Active Directory 網域所需的核心元件的指示。

- [網域名稱系統 (DNS)](../../../../networking/dns/dns-top.md):本主題提供的網域名稱系統 (DNS) 的概觀。 在 Windows Server 2016 中，DNS 會是您可以使用伺服器管理員] 或 [Windows PowerShell 命令來安裝伺服器角色。 如果您要安裝新的 Active Directory 樹系和網域，DNS 會自動與 Active Directory 安裝的樹系和網域的全域目錄伺服器。

- [Active Directory 憑證服務概觀](https://technet.microsoft.com/library/hh831740.aspx):本文件提供 Windows Server® 2012年中的 Active Directory 憑證服務 (AD CS) 的概觀。 AD CS 是一種伺服器角色，可以讓您為組織建立公開金鑰基礎結構 (PKI)，並提供公開金鑰密碼編譯、數位憑證以及數位簽章功能。

- [公開金鑰基礎結構設計指引](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx):此 wiki 提供指引設計公用金鑰基礎結構 (Pki)。 您設定 PKI 和憑證授權單位 (CA) 階層之前，您應該留意您的組織安全性原則和憑證實施 (準則 CPS)。

- [AD CS 逐步解說指南：兩層 PKI 階層部署](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx):本指南將逐步說明設定實驗室環境中的 Active Directory® 憑證服務 (AD CS) 的基本組態所需的步驟。 Windows Server® 2008 R2 中的 AD CS 提供建立及管理公開金鑰憑證用於採用公開金鑰技術的軟體安全性系統的可自訂服務。

- [網路原則伺服器 (NPS)](../../../../networking/technologies/nps/nps-top.md):本主題提供 Windows Server 2016 中的網路原則伺服器的概觀。 網路原則伺服器 (NPS) 可讓您建立並執行全組織網路存取原則，以用於連線要求驗證與授權。

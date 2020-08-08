---
title: Always On VPN 技術總覽
description: '本頁 provies 簡短介紹 Always On 的 VPN 技術，並提供詳細檔的連結。 '
ms.topic: article
ms.date: 11/05/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.openlocfilehash: 155d06657811878464d905a51f249cbc8ad3cfe2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958286"
---
# <a name="always-on-vpn-technology-overview"></a>Always On VPN 技術總覽

>適用于： Windows Server (半年通道) 、Windows Server 2016、Windows Server 2012 R2、Windows 10

- [**上一步：** 瞭解 Always On 的 VPN 增強功能](always-on-vpn-enhancements.md)
- [**下一步：** 瞭解 Always On VPN 的 advanced 功能](deploy/always-on-vpn-adv-options.md)

針對此部署，您必須安裝執行 Windows Server 2016 的新遠端存取服務器，以及修改一些部署的現有基礎結構。

下圖顯示部署 Always On VPN 所需的基礎結構。

![Always On VPN 基礎結構](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

此圖中描述的連線套裝程式含下列步驟：

1. 使用公用 DNS 伺服器，Windows 10 VPN 用戶端會針對 VPN 閘道的 IP 位址執行名稱解析查詢。

2. 使用 DNS 傳回的 IP 位址時，VPN 用戶端會將連線要求傳送至 VPN 閘道。

3. VPN 閘道也會設定為遠端驗證撥入使用者服務， (RADIUS) 用戶端;VPN RADIUS 用戶端會將連線要求傳送至組織/公司 NPS 伺服器，以進行連線要求處理。

4. NPS 伺服器會處理連線要求，包括執行授權和驗證，並決定是否要允許或拒絕連線要求。

5. NPS 伺服器會將存取-接受或拒絕存取回應轉送至 VPN 閘道。

6. 根據 VPN 伺服器從 NPS 伺服器接收到的回應，起始或終止連線。

如需有關上述圖中所描述之每個基礎結構元件的詳細資訊，請參閱下列各節。

>[!NOTE]
>如果您已經在網路上部署了其中一些技術，您可以使用此部署指引中的指示，針對此部署目的執行額外的技術設定。

## <a name="domain-name-system-dns"></a>網域名稱系統 (DNS)

內部和外部網域名稱系統 (DNS) 區域都是必要項，其假設內部區域是外部區域的委派子域 (例如 corp.contoso.com 和 contoso.com) 。

深入瞭解[網域名稱系統 (DNS) ](../../../../networking/dns/dns-top.md)或[核心網路指南](../../../../networking/core-network-guide/core-network-guide.md)。

>[!NOTE]
>其他 DNS 設計（例如，在不同的 DNS 區域內部和外部使用相同的功能變數名稱）) 或不相關的內部和外部 (網域 (（例如，也可能會有 contoso.com) ）。 如需部署分裂式 DNS 的詳細資訊，請參閱[使用適用于分裂式 Dns 部署的 Dns 原則](../../../../networking/dns/deploy/split-brain-DNS-deployment.md)。

## <a name="firewalls"></a>防火牆

請確定您的防火牆允許 VPN 和 RADIUS 通訊所需的流量正常運作。

如需詳細資訊，請參閱[設定 RADIUS 流量的防火牆](../../../../networking/technologies/nps/nps-firewalls-configure.md)。

## <a name="remote-access-as-a-ras-gateway-vpn-server"></a>作為 RAS 閘道 VPN 伺服器的遠端存取

在 Windows Server 2016 中，遠端存取服務器角色的設計是以路由器和遠端存取服務器的形式執行。因此，它支援廣泛的功能陣列。 針對此部署指導方針，您只需要這些功能的一小部分：支援 IKEv2 VPN 連線和 LAN 路由。

IKEv2 是網際網路工程任務要求批註7296中所述的 VPN 通道通訊協定。 IKEv2 的主要優點是它會容許基礎網路連線的中斷。 例如，如果連線暫時遺失，或使用者將用戶端電腦從一個網路移到另一個網路，則在重新建立網路連線時，IKEv2 會自動還原 VPN 連線，而不需要使用者介入。

藉由使用 RAS 閘道，您可以部署 VPN 連線，讓使用者可以從遠端存取您組織的網路和資源。 部署 Always On VPN 會在遠端電腦連線到網際網路時，維護用戶端與組織網路之間的持續連接。 使用 RAS 閘道時，您也可以在不同位置的兩部伺服器之間建立站對站 VPN 連線，例如主要辦公室和分公司，以及使用網路位址轉譯 (NAT) 讓網路內的使用者可以存取外部資源（例如網際網路）。 此外，RAS 閘道支援邊界閘道協定 (BGP) ，當您的遠端辦公室位置也有支援 BGP 的邊緣閘道時，它會提供動態路由服務。

您可以使用 Windows PowerShell 命令和遠端存取 Microsoft Management Console (MMC) ，來管理遠端存取服務 (RAS) 閘道。

## <a name="network-policy-server-nps"></a>網路原則伺服器 (NPS)

NPS 可讓您針對連線要求驗證和授權，建立並強制執行整個組織的網路存取原則。 當您使用 NPS 做為遠端驗證撥入使用者服務 (RADIUS) 伺服器時，您可以設定網路存取伺服器（例如 VPN 伺服器）做為 NPS 中的 RADIUS 用戶端。

您也可以設定 NPS 用來授權連線要求的網路原則，並且可以設定 RADIUS 帳戶處理，讓 NPS 將計量資訊記錄到本機硬碟上或 Microsoft SQL Server 資料庫中的記錄檔。

如需詳細資訊，請參閱[網路原則伺服器 (NPS)](../../../../networking/technologies/nps/nps-top.md)。

## <a name="active-directory-certificate-services"></a>Active Directory 憑證服務

CA) 伺服器 (憑證授權單位單位是執行 Active Directory 憑證服務的憑證授權單位單位。 VPN 設定需要 (PKI) 以 Active Directory 為基礎的公開金鑰基礎結構。

組織可以藉由將個人、裝置或服務的身分識別系結至對應的公開金鑰，來使用 AD CS 來加強安全性。 AD CS 也包含其他功能，讓您管理各種彈性環境中的憑證註冊與撤銷。 如需詳細資訊，請參閱[Active Directory 憑證服務總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831740(v=ws.11))和[公開金鑰基礎結構設計指導](https://techcommunity.microsoft.com/t5/ask-the-directory-services-team/designing-and-implementing-a-pki-part-i-design-and-planning/ba-p/396953)方針。

部署完成時，您將在 CA 上設定下列憑證範本。

- 使用者驗證憑證範本

- VPN 伺服器驗證憑證範本

- NPS 伺服器驗證憑證範本

### <a name="certificate-templates"></a>憑證範本

憑證範本可以讓您發行針對所選工作預先設定的憑證，大幅簡化管理憑證授權單位單位 (CA) 的工作。 [憑證範本] MMC 嵌入式管理單元可讓您執行下列工作。

- 檢視每個憑證範本的內容。

- 複製及修改憑證範本。

- 控制哪些使用者和電腦可以讀取範本及註冊憑證。

- 執行與憑證範本相關的其他管理工作。

憑證範本是企業憑證授權單位 (CA) 不可缺少的部份。 它們對於環境而言是重要的憑證原則元素，而且是憑證註冊、使用及管理的規則集和格式。

如需詳細資訊，請參閱[憑證範本](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730705(v=ws.11))。

### <a name="digital-server-certificates"></a>數位伺服器憑證

此部署指引提供使用 Active Directory 憑證服務 (AD CS) 的指示，以註冊憑證並自動註冊至遠端存取和 NPS 基礎結構伺服器。 AD CS 可讓您建立 (PKI) 的公開金鑰基礎結構，並為您的組織提供公開金鑰加密、數位憑證和數位簽章功能。

當您在網路上的電腦之間使用數位伺服器憑證進行驗證時，憑證會提供：

1. 透過加密的機密性。

2. 透過數位簽章的完整性。

3. 藉由將憑證金鑰與電腦網路上的電腦、使用者或裝置帳戶產生關聯，進行驗證。

如需詳細資訊，請參閱[Active Directory 憑證服務總覽](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831740(v=ws.11))。

## <a name="active-directory-domain-services-ad-ds"></a>Active Directory Domain Services (AD DS)

AD DS 提供一個分散式資料庫，用來儲存和管理網路資源的相關資訊，以及啟用目錄之應用程式的應用程式特定資料。 系統管理員可以使用 AD DS 將網路項目 (像是使用者、電腦以及其他裝置) 組織成階層式的內含項目結構。 階層式內含項目結構包含 Active Directory 樹系、樹系中的網域以及每個網域中的組織單位 (OU)。 執行 AD DS 的伺服器稱為網域控制站。

AD DS 包含受保護的可延伸驗證通訊協定所需的使用者帳戶、電腦帳戶和帳戶內容 (PEAP) 來驗證使用者認證，以及評估 VPN 連線要求的授權。 如需部署 AD DS 的詳細資訊，請參閱《 Windows Server 2016[核心網路指南》](../../../../networking/core-network-guide/Core-Network-Guide.md)。

完成此部署中的步驟時，您會在網域控制站上設定下列專案。

- 針對電腦和使用者在群組原則中啟用憑證自動註冊

- 建立 VPN 使用者群組

- 建立 VPN 伺服器群組

- 建立 NPS 伺服器群組

### <a name="active-directory-users-and-computers"></a>Active Directory 使用者及電腦

Active Directory 使用者和電腦是 AD DS 的元件，其中包含代表實體實體的帳戶，例如電腦、個人或安全性群組。 安全性群組是使用者或電腦帳戶的集合，系統管理員可將其管理為單一單位。 屬於特定群組的使用者和電腦帳戶稱為群組成員。

Active Directory 使用者和電腦中的使用者帳戶具有 NPS 在授權程式期間所評估的撥入內容-除非使用者帳戶的**網路存取權限**內容已設定為**透過 NPS 網路原則控制存取**。 這是所有使用者帳戶的預設設定。 不過，在某些情況下，此設定可能會有不同的設定，而導致使用者無法使用 VPN 進行連接。 若要防止這種可能性，您可以設定 NPS 伺服器忽略使用者帳戶撥入內容。

如需詳細資訊，請參閱[設定 NPS 以忽略使用者帳戶撥入屬性](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties)。

### <a name="group-policy-management"></a>群組原則管理

群組原則管理可讓使用者和電腦設定以目錄為基礎的變更和設定管理，包括安全性和使用者資訊。 您可以使用群組原則來定義使用者群組和電腦的設定。

使用群組原則，您可以指定登錄專案、安全性、軟體安裝、腳本、資料夾重新導向、遠端安裝服務，以及 Internet Explorer 維護的設定。 您所建立的群組原則設定會包含在群組原則物件 (GPO) 中。 藉由將 GPO 與所選 Active Directory 系統容器（網站、網域和 Ou）產生關聯，您可以將 GPO 的設定套用到這些 Active Directory 容器中的使用者和電腦。 若要管理整個企業的群組原則物件，您可以使用群組原則管理編輯器 Microsoft Management Console (MMC) 。

## <a name="windows-10-vpn-clients"></a>Windows 10 VPN 用戶端

除了伺服器元件之外，請確定您設定使用 VPN 的用戶端電腦執行的是 Windows 10 年度更新版 (版本 1607) 。 Windows 10 VPN 用戶端必須已加入 Active Directory 網域的網域。


Windows 10 VPN 用戶端是可高度設定的，並提供許多選項。 為了更清楚說明此案例所使用的特定功能，[表 1] 會識別此部署所參考的 VPN 功能類別和特定設定。 您將使用 VPNv2 設定服務提供者 (CSP 來設定這些功能的個別設定，) 稍後在此部署中討論。

表 1. 此部署中討論的 VPN 功能和設定

| VPN 功能     |     部署案例設定         |
|-----------------|-----------------------------------------------|
| 連線類型 |                 原生 IKEv2                  |
|     路由     |                分割通道                |
| 名稱解析 |  功能變數名稱資訊清單和 DNS 尾碼  |
|   各個    |    Always On 和信任的網路偵測    |
| 驗證  | PEAP-TLS 與 TPM-受保護的使用者憑證 |

>[!NOTE]
>PEAP-TLS 和 TPM 分別是「具有傳輸層安全性的受保護可延伸驗證通訊協定」和「信賴平臺模組」。

### <a name="vpnv2-csp-nodes"></a>VPNv2 CSP 節點

在此部署中，您會使用 ProfileXML VPNv2 CSP 節點來建立傳遞至 Windows 10 用戶端電腦的 VPN 設定檔。  (Csp) 的設定服務提供者是在 Windows 用戶端中公開各種管理功能的介面;在概念上，Csp 的運作方式類似于群組原則的運作方式。 每個 CSP 都有代表個別設定的設定節點。 也如同群組原則設定，您可以將 CSP 設定與登錄機碼、檔案、許可權等等結合。 類似于您使用群組原則管理編輯器來設定 Gpo)  (群組原則物件，您可以使用行動裝置管理 (MDM) 解決方案（例如 Microsoft Intune）來設定 CSP 節點。 Intune 這類 MDM 產品提供可在作業系統中設定 CSP 的易記設定選項。

![將行動裝置管理設定為 CSP 設定](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

不過，您無法透過使用者介面直接設定一些 CSP 節點， (UI) 例如 Intune 管理主控台。 在這些情況下，您必須手動設定「開放式行動聯盟」統一資源識別項 (OMA-URI) 設定。 您可以使用 OMA 裝置管理通訊協定來設定 OMA-URI， (OMA-URI) ，這是最新的 Apple、Android 和 Windows 裝置都支援的通用裝置管理規格。 只要遵守 OMA DM 規格，所有 MDM 產品都應該以相同的方式與這些作業系統互動。

Windows 10 提供許多 Csp，但此部署著重于使用 VPNv2 CSP 來設定 VPN 用戶端。 VPNv2 CSP 允許透過唯一的 CSP 節點，設定 Windows 10 中的每個 VPN 設定檔設定。 VPNv2 CSP 中也包含一個稱為*ProfileXML*的節點，可讓您設定一個節點中的所有設定，而不是個別進行。 如需 ProfileXML 的詳細資訊，請參閱此部署稍後的「ProfileXML 總覽」一節。 如需每個 VPNv2 CSP 節點的詳細資訊，請參閱[VPNV2 CSP](/windows/client-management/mdm/vpnv2-csp)。

## <a name="next-steps"></a>後續步驟

- [瞭解一些 advanced Always On VPN 功能](deploy/always-on-vpn-adv-options.md)

- [開始規劃您的 Always On VPN 部署](deploy/always-on-vpn-deploy-deployment.md)

## <a name="related-topics"></a>相關主題

- [Microsoft Azure 虛擬機器的 Microsoft 伺服器軟體支援](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)：本文討論在 Microsoft Azure 虛擬機器環境中執行 microsoft 伺服器軟體的支援原則， (基礎結構即服務) 。

- [遠端存取](../../Remote-Access.md)：本主題提供 Windows server 2016 中遠端存取服務器角色的總覽。

- [Windows 10 VPN 技術指南](/windows/access-protection/vpn/vpn-guide)：本指南會逐步引導您進行企業 VPN 解決方案中的 Windows 10 用戶端，以及如何設定部署的決策。 本指南參考 VPNv2 設定服務提供者 (CSP)，並使用 Microsoft Intune 和適用於 Windows 10 的「VPN 設定檔」範本來提供行動裝置管理 (MDM) 設定指示。

- [核心網路指南](../../../../networking/core-network-guide/Core-Network-Guide.md)：本指南提供如何在新樹系中規劃和部署全功能網路和新 Active Directory 網域所需之核心元件的指示。

- [網域名稱系統 (dns) ](../../../../networking/dns/dns-top.md)：本主題提供網域名稱系統 (dns) 的總覽。 在 Windows Server 2016 中，DNS 是一種伺服器角色，您可以使用伺服器管理員或 Windows PowerShell 命令來安裝它。 如果您要安裝新的 Active Directory 樹系和網域，則會自動安裝 DNS，Active Directory 做為樹系和網域的全域目錄伺服器。

- [Active Directory 憑證服務總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831740(v=ws.11))：本檔提供 Windows Server 2012 中 (AD CS) Active Directory 憑證服務的總覽 &reg; 。 AD CS 是一種伺服器角色，可以讓您為組織建立公開金鑰基礎結構 (PKI)，並提供公開金鑰密碼編譯、數位憑證以及數位簽章功能。

- [公開金鑰基礎結構設計指引](https://techcommunity.microsoft.com/t5/ask-the-directory-services-team/designing-and-implementing-a-pki-part-i-design-and-planning/ba-p/396953)：此論壇提供設計 (pki) 之公開金鑰基礎結構的指導方針。  (CA) 階層設定 PKI 和憑證授權單位單位之前，請留意貴組織的安全性原則和憑證實務聲明 (CPS) 。

- [Active Directory 憑證服務總覽](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831740(v=ws.11))：此逐步指南說明 &reg; 在實驗室環境中設定 Active Directory 憑證服務 (AD CS) 的基本設定所需的步驟。 Windows Server &reg; 2008 R2 中的 AD CS 提供了可自訂的服務，可用來建立及管理採用公開金鑰技術的軟體安全性系統中所使用的公開金鑰憑證。

- [網路原則伺服器 (NPS) ](../../../../networking/technologies/nps/nps-top.md)：本主題提供 Windows Server 2016 中網路原則伺服器的總覽。 網路原則伺服器 (NPS) 可讓您建立並執行全組織網路存取原則，以用於連線要求驗證與授權。

---
title: Always On VPN 技術概觀
description: '此頁面 provies 與詳細文件的連結的 Always On VPN 技術的簡要概觀。 '
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: fd3f7c6ca8555e270aabf04bbee6800ed284080c
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031322"
---
# Always On VPN 技術概觀
>適用於： Windows Server （半年度管道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #171; [**上一步：** 深入了解 Always On VPN 增強功能](always-on-vpn-enhancements.md)<br>
& #187; [**下一步：** 深入了解 Always On VPN 的進階功能](deploy/always-on-vpn-adv-options.md)

如需此部署中，您必須安裝新的遠端存取伺服器執行 Windows Server 2016 中，以及修改您現有的基礎結構，適用於部署的一些。

下圖顯示，才能部署 Always On VPN 基礎結構。

![Always On VPN 基礎結構](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

在此圖例中所述的連線程序包含下列步驟：

1. 使用公用 DNS 伺服器，Windows 10 VPN 用戶端會執行名稱解析查詢的 VPN 閘道 IP 位址。

2. 使用 DNS 所傳回的 IP 位址，VPN 用戶端會傳送至 VPN 閘道連線要求。

3. VPN 閘道也已設定為 [遠端驗證撥入使用者服務 \(RADIUS\) 的用戶端。VPN RADIUS 用戶端會連線要求傳送至組織/公司 NPS 伺服器進行連線要求處理。

4. NPS 伺服器處理連線要求，包括執行授權和驗證，並決定是否要允許或拒絕連線要求。

5. NPS 伺服器會轉送 VPN 閘道的存取權接受或拒絕存取回應。

6. 起始或終止連接根據 VPN 伺服器收到來自 NPS 伺服器的回應。

如需每個上述圖例所示的基礎結構元件的詳細資訊，請參閱下列各節。

>[!NOTE]
>如果您已經有一些您網路上部署這些技術，您可以使用此部署指導中的指示為此部署目的執行額外的設定的技術。

## 網域名稱系統 (DNS)

內部和外部網域名稱系統 (DNS) 區域是必要的即假設內部區域是委派子網域的外部的區域 （例如，corp.contoso.com 和 contoso.com）。

了解更多關於[網域名稱系統 (DNS)](../../../../networking/dns/dns-top.md)或[核心網路指南](../../../../networking/core-network-guide/core-network-guide.md)。




>[!NOTE] 
>其他 DNS 設計，例如拆分式 DNS 部署 （使用相同的網域名稱內部和外部個別的 DNS 區域中），或不相關的內部和外部網域 （例如，contoso.local 和 contoso.com） 也是可能。 如需部署拆分式 DNS 部署的詳細資訊，請參閱[使用 DNS 原則進行分割 /-大腦 DNS 部署](../../../../networking/dns/deploy/split-brain-DNS-deployment.md)。

## 防火牆

請確定您的防火牆允許流量所需的 VPN 和半徑通訊正常運作。

如需詳細資訊，請參閱[設定 RADIUS 流量的防火牆](../../../../networking/technologies/nps/nps-firewalls-configure.md)。

## 遠端存取做為 RAS 閘道 VPN 伺服器

在 Windows Server 2016 中，以及同時路由器 」 和 「 遠端存取伺服器; 執行設計遠端存取伺服器角色因此，它支援各種功能。 針對此部署指導方針，您需要這些功能一小部分： 支援 IKEv2 VPN 連線，以及 LAN 路由。

IKEv2 是 VPN 通道通訊協定的註解 7296 網際網路 [工程任務推動要求中所述。 IKEv2 的主要優點是它可容許基礎的網路連線中斷。 例如，如果連線暫時中斷或使用者從一個網路用戶端電腦移到另一個，IKEv2 自動還原 VPN 連線時重新建立網路連線，不需要使用者介入。

藉由使用 RAS 閘道，您可以部署 VPN 連線，為使用者提供遠端存取您組織的網路和資源。 部署 Always On VPN 用戶端與您的組織網路之間持續連線，只要會維護遠端電腦連線到網際網路。 使用 RAS 閘道，您可以也站台之間建立 VPN 連線兩部伺服器位於不同的位置，例如您的主要辦公室之間分公司，並使用網路位址轉譯 \(NAT\)，讓網路內的使用者可以存取外部資源，例如網際網路。 此外，RAS 閘道支援邊界閘道通訊協定 (BGP)，當您的遠端辦公室位置也有支援 BGP edge 閘道提供動態路由服務。

您可以使用 Windows PowerShell 命令和遠端存取 Microsoft Management Console (MMC) 來管理遠端存取服務 (RAS) 閘道。



## 網路原則伺服器 (NPS)

NPS 可讓您建立並執行全組織網路連線要求驗證與授權的存取原則。 當您使用 NPS 做為遠端驗證撥入使用者服務 (RADIUS) 伺服器時，您可以設定網路存取伺服器，例如 VPN 伺服器作為 RADIUS 用戶端在 NPS 中。

您也可以設定 NPS 使用授權連線要求的網路原則，並使 NPS 帳戶處理資訊記錄到記錄檔在本機硬碟上或在 Microsoft SQL Server 資料庫中，您可以設定 RADIUS 帳戶處理。

如需詳細資訊，請參閱[網路原則伺服器 (NPS)](../../../../networking/technologies/nps/nps-top.md)。


## Active Directory 憑證服務

執行 Active Directory 憑證服務的憑證授權單位憑證授權單位 (CA) 伺服器。 VPN 設定需要 Active Directory 型公開金鑰基礎結構 (PKI)。

組織可以使用 AD CS 來進行加強安全性繫結到對應的公開金鑰的個人、 裝置或服務的身分識別。 AD CS 也包含可讓您管理憑證註冊和撤銷各種不同的可調整的環境中的功能。 如需詳細資訊，請參閱[Active Directory 憑證服務概觀](https://technet.microsoft.com/library/hh831740.aspx)和[公開金鑰金鑰基礎結構設計指導方針](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx)。

期間完成的部署，您將 CA 上設定下列憑證範本。

-   使用者驗證憑證範本

-   VPN 伺服器驗證憑證範本

-   NPS 伺服器驗證憑證範本

### 憑證範本

憑證範本可大幅簡化管理憑證授權單位 (CA) 的工作，藉由允許您為所選的工作來預先設定的發行憑證。 憑證範本 MMC 嵌入式管理單元可讓您執行下列工作。

-   檢視每個憑證範本的內容。

-   複製並修改憑證範本。

-   控制哪些使用者和電腦可以讀取範本和註冊憑證。

-   執行與憑證範本相關的其他管理工作。

憑證範本是企業憑證授權單位 (CA) 中不可或缺的一部分。 它們是一項重要元素的憑證原則的環境，也就是組規則和格式的憑證註冊、 使用及管理。

如需詳細資訊，請參閱[憑證範本](https://technet.microsoft.com/library/cc730705.aspx)。

### 數位伺服器憑證

本部署指南提供使用 Active Directory 憑證服務 (AD CS) 到註冊和自動註冊至遠端存取和 NPS 基礎結構伺服器憑證的指示。 AD CS 可讓您建置公開金鑰基礎結構 (PKI)，並為您的組織提供公開金鑰密碼編譯、 數位憑證及數位簽章功能。

當您使用的數位伺服器憑證進行驗證您網路上的電腦之間時，會提供的憑證：

1.  透過加密的機密性。

2.  透過數位簽章的完整性。

3.  藉由建立憑證金鑰與電腦網路上的電腦、 使用者或裝置帳戶的關聯的驗證。

如需詳細資訊，請參閱[AD CS 逐步解說指南： 兩個層 PKI 階層部署](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)。

## Active Directory 網域服務 (AD DS)

AD DS 提供分散式的資料庫來儲存和從目錄啟用應用程式管理網路資源和特定應用程式資料的相關資訊。 系統管理員可用來將元素的網路，例如使用者、 電腦及其他裝置，組織成階層式內含項目結構 AD DS。 階層式內含項目結構包含 Active Directory 樹系網域、 樹系和組織單位 (Ou) 中的每一個網域。 執行 AD DS 的伺服器稱為 「 網域控制站。

AD DS 包含使用者帳戶、 電腦帳戶和帳戶屬性所需的受保護的可延伸驗證通訊協定 (PEAP)，以驗證使用者認證，並評估 VPN 連線要求的授權。 如需部署 AD DS 的資訊，請參閱 Windows Server 2016[核心網路指南](../../../../networking/core-network-guide/Core-Network-Guide.md)。



期間完成此部署中的步驟，您將網域控制站上設定下列項目。

-   啟用群組原則中的電腦與使用者的憑證自動註冊

-   建立 VPN 使用者群組

-   建立 VPN 伺服器群組

-   建立 NPS 伺服器群組

### Active Directory 使用者和電腦

Active Directory 使用者和電腦是一個元件的 AD DS 包含代表實體，例如在電腦、 個人或安全性群組的帳戶。 安全性群組是一組系統管理員可以管理以單一單位的使用者或電腦帳戶。 屬於特定群組的使用者和電腦帳戶稱為群組成員。

在 Active Directory 使用者和電腦的使用者帳戶具有撥入屬性，NPS 評估授權程序的期間，除非使用者帳戶的**網路存取權限**屬性設定為透過 NPS 網路原則來控制存取****. 這是所有使用者帳戶的預設設定。 不過，在某些情況下，此設定可能會有不同的設定會阻止使用者使用 VPN 連線。 若要防止這個可能性，您可以設定 NPS 伺服器以略過使用者帳戶撥入內容。

如需詳細資訊，請參閱[設定 NPS 略過使用者帳戶撥入內容](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties)。



### 群組原則管理

群組原則管理啟用目錄型變更及設定的管理使用者和電腦的設定，包括安全性和使用者的資訊。 您可以使用群組原則來定義群組的使用者和電腦的設定。

使用群組原則，您可以指定的登錄項目、 安全性、 軟體安裝、 指令碼、 資料夾重新導向、 遠端安裝服務及 Internet Explorer 維護設定。 您所建立的群組原則設定被包含在群組原則物件 (GPO)。 選取 Active Directory 系統容器關聯 GPO，藉此 — 網站、 網域及 Ou — 您可以將 GPO 設定套用到使用者，以及這些 Active Directory 容器中的電腦。 若要在企業管理群組原則物件，您可以使用群組原則管理編輯器 Microsoft Management Console (MMC)。


## Windows 10 VPN 用戶端

除了伺服器元件，請確定您設定為使用 VPN 用戶端電腦執行 windows 10 年度更新版 (version1607)。 Windows 10 VPN 用戶端必須是您的 Active Directory 網域加入網域。


Windows 10 VPN 用戶端是高度可設定，並提供許多選項。 為了更清楚說明這個案例中使用的特定功能，Table1 識別參考此部署 VPN 功能類別和特定的設定。 您將會使用 VPNv2 設定服務提供者 (CSP) 稍後討論這種部署設定這些功能的個別設定。 

表 1. VPN 功能與此部署中所討論的設定

| **VPN 功能** | **部署案例設定**         |
|-----------------|-----------------------------------------------|
| 連線類型 | 原生 IKEv2                                  |
| 路由         | 分割通道                               |
| 名稱解析 | 網域名稱的資訊清單和 DNS 尾碼   |
| 觸發      | 一律開啟且受信任的網路偵測       |
| 驗證  | PEAP TLS 與 TPM 保護使用者的憑證 |
---

>[!NOTE] 
>PEAP-TLS 和 TPM 分別是 「 受保護的可延伸驗證通訊協定與傳輸層安全性 」 和 「 信賴平台模組 」。

### VPNv2 CSP 節點

在此部署中，您可以使用 ProfileXML VPNv2 CSP 節點來建立會傳遞到 windows 10 用戶端電腦在 VPN 設定檔。 設定服務提供者 (Csp) 會公開各種不同的管理功能，Windows 用戶端; 內的介面在概念上，Csp 工作群組原則的運作方式類似。 每個雲端解決方案提供者有代表個別設定的設定節點。 也像群組原則設定，您可以繫結到登錄機碼、 檔案、 權限，以及等等的雲端解決方案提供者設定。 類似於您如何使用群組原則管理編輯器來設定群組原則物件 (Gpo)，您設定雲端解決方案提供者的節點使用行動裝置管理 (MDM) 解決方案，例如 Microsoft Intune。 像 Intune MDM 產品提供可在作業系統中設定雲端解決方案提供者方便使用的設定選項。

![雲端解決方案提供者設定至行動裝置管理](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

不過，您無法設定一些直接透過像是 Intune 管理主控台使用者介面 (UI) 的雲端解決方案提供者節點。 在這些情況下，您必須設定的開放行動聯盟統一資源識別項 (OMA-URI) 設定以手動方式。 您可以使用 OMA 裝置管理通訊協定 (OMA-DM)，大部分新型 Apple、 Android 和 Windows 裝置都支援通用裝置管理規格，以設定 OMA Uri。 只要它們都遵守 OMA DM 規格，所有的 MDM 產品應該與互動，這些作業系統相同的方式。

Windows 10 提供許多 Csp，但此部署將焦點放在使用 VPNv2 CSP 來設定 VPN 用戶端。 VPNv2 CSP 會允許在 windows 10 中的每個 VPN 設定檔設定的設定，透過獨特的雲端解決方案提供者節點。 VPNv2 CSP 中也包含是稱為*ProfileXML*，這可讓您設定的所有設定的節點的某一個節點，而非個別。 如需有關 ProfileXML 的詳細資訊，請參閱此部署在稍後的 「 ProfileXML 概觀 」 一節。 如需有關每個 VPNv2 CSP 節點的詳細資訊，請參閱[VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp)。



## 後續步驟

- [深入了解的一些進階 Always On VPN 功能](deploy/always-on-vpn-adv-options.md)

- [開始規劃 Always On VPN 部署](deploy/always-on-vpn-deploy-deployment.md)


---

## 相關主題
- [Microsoft 伺服器軟體支援 Microsoft Azure 的虛擬機器](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)： 這篇文章將討論 Microsoft Azure 虛擬機器環境 （基礎結構做為-為服務） 中執行 Microsoft 伺服器軟體的支援原則。

- [遠端存取](../../Remote-Access.md)： 本主題提供 Windows Server 2016 中的遠端存取伺服器角色的概觀。

- [Windows 10 VPN 技術指南](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide)： 本指南將引導您完成您的企業 VPN 解決方案，以及如何設定您的部署中進行的 Windows 10 用戶端的決策。 本指南參考 VPNv2 設定服務提供者 (CSP)，並使用 Microsoft Intune 和適用於 Windows10 的「VPN 設定檔」範本來提供行動裝置管理 (MDM) 設定指示。

- [核心網路指南](../../../../networking/core-network-guide/Core-Network-Guide.md)： 本指南提供如何規劃及部署完全正常運作的網路和新的樹系中新的 Active Directory 網域所需的核心元件相關指示。

- [網域名稱系統 (DNS)](../../../../networking/dns/dns-top.md): 本主題提供的網域名稱系統 (DNS) 的概觀。 在 Windows Server 2016 中，DNS 會是您可以使用伺服器管理員或 Windows PowerShell 命令來安裝伺服器角色。 如果您要安裝新的 Active Directory 樹系和網域，DNS 會自動與 Active Directory 安裝為樹系和網域的全域型錄伺服器。 

- [Active Directory 憑證服務概觀](https://technet.microsoft.com/library/hh831740.aspx)： 這份文件提供 Windows Server® 2012年中的 Active Directory 憑證服務 (AD CS) 的概觀。 AD CS 是可讓您建置公開金鑰基礎結構 (PKI)，並為您的組織提供公開金鑰密碼編譯、 數位憑證及數位簽章功能的伺服器角色。

- [公用金鑰基礎結構設計指導方針](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx)： 這個 wiki 上設計公開金鑰基礎結構 (Pki) 提供的指導方針。 設定 PKI 和憑證授權單位 (CA) 階層之前，您應該要注意您組織的安全性原則和憑證的做法聲明 (CPS) 的項目。

- [AD CS 逐步解說指南： 兩個層 PKI 階層部署](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)： 本指南將逐步說明設定 Active Directory® 憑證服務 (AD CS) 在實驗室環境中的基本設定所需的步驟。 在 Windows Server® 2008 R2 中的 AD CS 提供可自訂的服務建立和管理軟體安全性系統大規模公用的關鍵技術中使用公開金鑰憑證。

- [網路原則伺服器 (NPS)](../../../../networking/technologies/nps/nps-top.md): 本主題提供 Windows Server 2016 中的網路原則伺服器的概觀。 網路原則伺服器 (NPS) 可讓您建立並執行全組織網路存取原則，以用於連線要求驗證與授權。 

---

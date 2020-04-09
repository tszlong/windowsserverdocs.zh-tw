---
title: 部署 Always On VPN
description: 本主題提供在 Windows Server 2016 中部署 Always On VPN 的詳細指示。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.openlocfilehash: 0889c8a3472509ec3e3a9d013ba649df7ac63d24
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860051"
---
# <a name="deploy-always-on-vpn"></a>部署 Always On VPN

>適用于： Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows 10

- [**上一步：** 瞭解 Always On VPN advanced 功能](always-on-vpn-adv-options.md)
- [**下一步：** 步驟1。開始規劃 Always On VPN 部署](always-on-vpn-deploy-planning.md)

在本節中，您將瞭解針對已加入網域的遠端 Windows 10 用戶端電腦部署 Always On VPN 連線的工作流程。 如果您想要**設定條件式存取**來微調 vpn 使用者存取資源的方式，請參閱[使用 Azure AD Vpn 連線的條件式存取](../../ad-ca-vpn-connectivity-windows10.md)。 若要深入瞭解使用 Azure AD 進行 VPN 連線的條件式存取，請參閱[Azure Active Directory 中的條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。 

下圖說明部署 Always On VPN 時，不同案例的工作流程程式：

[Always On VPN 部署工作流程的 ![流程圖](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow.png)

> [!IMPORTANT]
> 在此部署中，不需要您的基礎結構伺服器（例如執行 Active Directory Domain Services 的電腦、Active Directory 憑證服務和網路原則伺服器）正在執行 Windows Server 2016。 您可以針對基礎結構伺服器以及執行遠端存取的伺服器，使用舊版的 Windows Server，例如 Windows Server 2012 R2。

## <a name="step-1-plan-the-always-on-vpn-deployment"></a>[步驟1。規劃 Always On VPN 部署](always-on-vpn-deploy-planning.md)

在此步驟中，您會開始規劃和準備您的 Always On VPN 部署。 在您打算使用做為 VPN 伺服器的電腦上安裝遠端存取服務器角色之前。 適當規劃之後，您可以部署 Always On VPN，並選擇性地使用 Azure AD 設定 VPN 連線的條件式存取。

## <a name="step-2-configure-the-always-on-vpn-server-infrastructure"></a>[步驟2。設定 Always On VPN 伺服器基礎結構](vpn-deploy-server-infrastructure.md)

在此步驟中，您會安裝並設定支援 VPN 所需的伺服器端元件。 伺服器端元件包括設定 PKI，以散發使用者、VPN 伺服器和 NPS 伺服器所使用的憑證。  您也可以設定 RRAS 來支援 IKEv2 連線，以及執行 VPN 連線授權的 NPS 伺服器。

若要設定伺服器基礎結構，您必須執行下列工作：

- **在使用 Active Directory Domain Services 設定的伺服器上：** 針對電腦和使用者在群組原則中啟用憑證自動註冊、建立 VPN 使用者群組、VPN 伺服器群組和 NPS 伺服器群組，並將成員新增至每個群組。
- **在 Active Directory 憑證伺服器 CA 上：** 建立使用者驗證、VPN 伺服器驗證和 NPS 伺服器驗證憑證範本。
- **在已加入網域的 Windows 10 用戶端上：** 註冊並驗證使用者憑證。

## <a name="step-3-configure-the-remote-access-server-for-always-on-vpn"></a>[步驟3。設定 Always On VPN 的遠端存取服務器](vpn-deploy-ras.md)

在此步驟中，您會設定遠端存取 VPN 以允許 IKEv2 VPN 連線、拒絕來自其他 VPN 通訊協定的連線，以及指派靜態 IP 位址池來發佈 IP 位址，以連線授權的 VPN 用戶端。

若要設定 RAS，您必須執行下列工作：

- 註冊並驗證 VPN 伺服器證書
- 安裝和設定遠端存取 VPN

## <a name="step-4-install-and-configure-the-nps-server"></a>[步驟4。安裝和設定 NPS 伺服器](vpn-deploy-nps.md)

在此步驟中，您會使用 Windows PowerShell 或伺服器管理員新增角色及功能嚮導 來安裝網路原則伺服器（NPS）。 您也可以設定 NPS，針對它從 VPN 伺服器接收的連線要求處理所有的驗證、授權和帳戶管理責任。

若要設定 NPS，您必須執行下列工作：

- 在 Active Directory 中註冊 NPS 伺服器
- 設定 NPS 伺服器的 RADIUS 帳戶處理
- 新增 VPN 伺服器做為 NPS 中的 RADIUS 用戶端
- 在 NPS 中設定網路原則
- 自動註冊 NPS 伺服器憑證

## <a name="step-5-configure-dns-and-firewall-settings-for-always-on-vpn"></a>[步驟5。設定 Always On VPN 的 DNS 和防火牆設定](vpn-deploy-dns-firewall.md)

在此步驟中，您會設定 DNS 和防火牆設定。 當遠端 VPN 用戶端連線時，它們會使用您的內部用戶端所使用的相同 DNS 伺服器，讓它們能夠以與您的其他內部工作站相同的方式來解析名稱。 

## <a name="step-6-configure-windows-10-client-always-on-vpn-connections"></a>[步驟6。設定 Windows 10 用戶端 Always On VPN 連線](vpn-deploy-client-vpn-connections.md)

在此步驟中，您會將 Windows 10 用戶端電腦設定為使用 VPN 連線與該基礎結構進行通訊。 您可以使用數種技術來設定 Windows 10 VPN 用戶端，包括 Windows PowerShell、Microsoft Endpoint Configuration Manager 和 Intune。 這三個都需要 XML VPN 設定檔來設定適當的 VPN 設定。

## <a name="step-7-optional-configure-conditional-access-for-vpn-connectivity"></a>[步驟7：選擇性設定 VPN 連線能力的條件式存取](../../ad-ca-vpn-connectivity-windows10.md)

在此選擇性步驟中，您可以微調授權的 VPN 使用者存取資源的方式。 透過 VPN 連線 Azure AD 的條件式存取，您可以協助保護 VPN 連接。 條件式存取是以原則為基礎的評估引擎，可讓您為任何 Azure AD 連線的應用程式建立存取規則。 如需詳細資訊，請參閱[Azure Active Directory （Azure AD）條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。

## <a name="next-step"></a>後續步驟

[步驟1。規劃 Always On VPN 部署](always-on-vpn-deploy-planning.md)：在您規劃用來做為 VPN 伺服器的電腦上安裝遠端存取服務器角色之前。 適當規劃之後，您可以部署 Always On VPN，並選擇性地使用 Azure AD 設定 VPN 連線的條件式存取。  

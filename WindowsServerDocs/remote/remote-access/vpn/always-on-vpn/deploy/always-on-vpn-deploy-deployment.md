---
title: 部署 Always On VPN
description: 本主題提供部署一律開啟 」 VPN Windows Server 2016 中的詳細的的指示。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6849d8547885b10fb32a133f09a82b6f133a535e
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749653"
---
# <a name="deploy-always-on-vpn"></a>部署 Always On VPN

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows 10

- [**前一個：** 深入了解永遠開啟 」 VPN 的進階功能](always-on-vpn-adv-options.md)
- [**下一步:** 步驟 1.開始規劃一律開啟 」 VPN 部署](always-on-vpn-deploy-planning.md)

在本節中，您會了解部署遠端已加入網域的 Windows 10 用戶端電腦一律開啟 」 VPN 連線的工作流程。 如果您想要**設定條件式存取**微調 VPN 使用者如何存取您的資源，請參閱[VPN 連線使用 Azure AD 條件式存取](../../ad-ca-vpn-connectivity-windows10.md)。 若要深入了解使用 Azure AD 的 VPN 連線能力的條件式存取，請參閱[Azure Active Directory 中的條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。 

部署 一律開啟 」 VPN 時下, 圖將說明不同的案例的工作流程程序：

![一律開啟 」 VPN 部署工作流程的流程圖表](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)

>[!IMPORTANT]
>針對此部署，它不是您的基礎結構伺服器，例如執行 Active Directory 網域服務、 Active Directory 憑證服務，以及網路原則伺服器的電腦執行 Windows Server 2016 的需求。 針對基礎結構伺服器和執行遠端存取伺服器，您可以使用舊版的 Windows Server，例如 Windows Server 2012 R2。

## <a name="step-1-plan-the-always-on-vpn-deploymentalways-on-vpn-deploy-planningmd"></a>[步驟 1.規劃 Always On VPN 部署](always-on-vpn-deploy-planning.md)

在此步驟中，您開始規劃及準備您的一律開啟 」 VPN 部署。 您的電腦上安裝遠端存取伺服器角色之前您打算使用做為 VPN 伺服器。 之後適當的規劃中，您可以部署一律開啟 」 VPN，並選擇性地設定 VPN 連線使用 Azure AD 條件式存取。

## <a name="step-2-configure-the-always-on-vpn-server-infrastructurevpn-deploy-server-infrastructuremd"></a>[步驟 2.設定 Always On VPN 伺服器基礎結構](vpn-deploy-server-infrastructure.md)

在此步驟中，您可以安裝及設定支援的 VPN 所需的伺服器端元件。 伺服器端元件包括設定將使用者、 VPN 伺服器和 NPS 伺服器所使用的憑證的 PKI。  您也可以設定 RRAS，才能支援 IKEv2 連線和 VPN 連線的執行授權的 NPS 伺服器。

若要設定的伺服器基礎結構，您必須執行下列工作：

- **在伺服器上使用 Active Directory 網域服務設定：** 啟用群組原則中的電腦和使用者的憑證自動註冊、 建立 VPN 使用者群組、 VPN 伺服器群組和 NPS 的伺服器群組，並將成員加入至每個群組。
- **在 Active Directory 憑證伺服器 CA:** 建立的使用者驗證、 VPN 伺服器驗證，以及 NPS 伺服器驗證憑證範本。
- **已加入網域的 Windows 10 用戶端：** 註冊和驗證使用者憑證。

## <a name="step-3-configure-the-remote-access-server-for-always-on-vpnvpn-deploy-rasmd"></a>[步驟 3.設定 Always On VPN 的遠端存取伺服器](vpn-deploy-ras.md)

在此步驟中，您可以設定允許 IKEv2 VPN 連線，拒絕來自其他 VPN 通訊協定，連線將發行的 IP 位址的靜態 IP 位址集區指派給已授權的 VPN 用戶端連線的遠端存取 VPN。

若要設定 RAS，您必須執行下列工作：

- 註冊並驗證 VPN 伺服器憑證
- 安裝和設定遠端存取 VPN

## <a name="step-4-install-and-configure-the-nps-servervpn-deploy-npsmd"></a>[步驟 4.安裝並設定 NPS 伺服器](vpn-deploy-nps.md)

在此步驟中，您可以安裝網路原則伺服器 (NPS) 藉由使用 Windows PowerShell 或伺服器管理員新增角色和功能精靈。 您也可以設定 NPS 來處理所有的驗證、 授權和帳戶管理職責，收到來自 VPN 伺服器的連線要求。

若要設定 NPS，您必須執行下列工作：

- 在 Active Directory 中登錄 NPS 伺服器
- 設定 RADIUS 帳戶處理的 NPS 伺服器
- 在 NPS 中的 RADIUS 用戶端將 VPN 伺服器
- 在 NPS 中設定網路原則
- 自動註冊 NPS 伺服器憑證

## <a name="step-5-configure-dns-and-firewall-settings-for-always-on-vpnvpn-deploy-dns-firewallmd"></a>[步驟 5.設定 DNS 和防火牆設定 Always On VPN](vpn-deploy-dns-firewall.md)

在此步驟中，您設定 DNS 和防火牆設定。 當遠端 VPN 用戶端連線時，它們會使用您的內部用戶端使用，同一個 DNS 伺服器，以便讓它們以在您的內部工作站的其餘部分相同的方式來解析名稱集。 

## <a name="step-6-configure-windows-10-client-always-on-vpn-connectionsvpn-deploy-client-vpn-connectionsmd"></a>[步驟 6.設定 Windows 10 用戶端 Always On VPN 連線](vpn-deploy-client-vpn-connections.md)

在此步驟中，您可以設定 Windows 10 用戶端電腦通訊的 VPN 連線與該基礎結構。 若要設定 Windows 10 VPN 用戶端，包括 Windows PowerShell、 System Center Configuration Manager 和 Intune，您可以使用數種技術。 這三個需要 XML VPN 設定檔來設定適當的 VPN 設定。

## <a name="step-7-optional-configure-conditional-access-for-vpn-connectivityad-ca-vpn-connectivity-windows10md"></a>[步驟 7.（選擇性）設定 VPN 連線能力的條件式存取](../../ad-ca-vpn-connectivity-windows10.md)

在此選擇性步驟中，您可以微調如何授權的 VPN 使用者存取您的資源。 使用 VPN 連線能力的 Azure AD 條件式存取，您可以協助保護 VPN 連線。 條件式存取是以原則為基礎的評估引擎，可讓您建立存取規則適用於任何 Azure AD 連線應用程式。 如需詳細資訊，請參閱 < [Azure Active Directory (Azure AD) 條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。

## <a name="next-step"></a>後續步驟

[步驟 1.計劃一律開啟 」 VPN 部署](always-on-vpn-deploy-planning.md):您的電腦上安裝遠端存取伺服器角色之前您打算使用做為 VPN 伺服器。 之後適當的規劃中，您可以部署一律開啟 」 VPN，並選擇性地設定 VPN 連線使用 Azure AD 條件式存取。  

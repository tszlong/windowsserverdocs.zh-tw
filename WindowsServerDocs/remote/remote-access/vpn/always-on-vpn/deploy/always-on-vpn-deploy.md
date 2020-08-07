---
title: Windows Server 和 Windows 10 的 Always On VPN 部署
description: 您可以使用此部署，透過 Windows Server 2016 或更新版本中的遠端存取，以及適用于 Windows 10 用戶端電腦的 Always On VPN 設定檔，為遠端員工部署 Always On 虛擬私人網路 (VPN) 連接。
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.openlocfilehash: bc7608d71dcb2bce19138fea18d6de23cda97d35
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963784"
---
# <a name="always-on-vpn-deployment-for-windows-server-and-windows-10"></a>Windows Server 和 Windows 10 的 Always On VPN 部署

>適用于： Windows Server (半年通道) 、Windows Server 2016、Windows Server 2012 R2、Windows 10

- [**上一步：** 遠端存取](../../../Remote-Access.md)<br>
- [**下一步：** 瞭解 Always On 的 VPN 特性和功能](../../vpn-map-da.md)

Always On VPN 為遠端存取提供單一且一致的解決方案，並支援已加入網域、未加入網域的 (workgroup) 或已加入 Azure AD 的裝置，甚至是個人擁有的裝置。 使用 Always On VPN，連線類型不必是專用使用者或裝置，但可以是兩者的組合。 例如，您可以為遠端裝置管理啟用裝置驗證，然後再啟用使用者驗證以連線到內部公司站台及服務。

## <a name="prerequisites"></a>必要條件

您很有可能已部署可用於部署 Always On VPN 的技術。 除了 DC/DNS 伺服器以外，Always On VPN 部署需要 NPS (RADIUS) 伺服器、憑證授權單位單位 (CA) 伺服器，以及遠端存取 (路由/VPN) 伺服器。 設定基礎結構之後，您必須註冊用戶端，然後透過數個網路變更，安全地將用戶端連線到內部部署。

- Active Directory 網域基礎結構，包括一或多個網域名稱系統 (DNS) 伺服器。 內部和外部網域名稱系統 (DNS) 區域都是必要項，其假設內部區域是外部區域的委派子域 (例如 corp.contoso.com 和 contoso.com) 。
- Active Directory 為基礎的公開金鑰基礎結構 (PKI) 和 Active Directory 憑證服務 (AD CS) 。
- [虛擬] 或 [實體]、[現有] 或 [新增] 的伺服器，以安裝網路原則伺服器 (NPS) 。 如果您的網路上已有 NPS 伺服器，您可以修改現有的 NPS 伺服器設定，而不是新增伺服器。
- 遠端存取作為 RAS 閘道 VPN 伺服器，其中包含支援 IKEv2 VPN 連線和 LAN 路由的一小部分功能。
- 包含兩個防火牆的周邊網路。  請確定您的防火牆允許 VPN 和 RADIUS 通訊所需的流量正常運作。 如需詳細資訊，請參閱[ALWAYS ON VPN 技術總覽](../always-on-vpn-technology-overview.md)。
- 實體伺服器或虛擬機器 (您周邊網路上的 VM) ，其中包含兩個實體 Ethernet 網路介面卡，可將遠端存取安裝為 RAS 閘道 VPN 伺服器。 Vm 需要虛擬 LAN (主機的 VLAN) 。
- 至少需要 Administrators 的成員資格或同等許可權。
- 請閱讀本指南的規劃一節，以確保您在執行部署之前已準備好進行這項部署。
- 請參閱設計和部署指南，以瞭解所使用的各項技術。 這些指南可協助您判斷部署案例是否提供貴組織網路所需的服務和設定。 如需詳細資訊，請參閱[ALWAYS ON VPN 技術總覽](../always-on-vpn-technology-overview.md)。
- 您選擇用來部署 Always On VPN 設定的管理平臺，因為 CSP 不是廠商特有的。

>[!IMPORTANT]
>在此部署中，不需要您的基礎結構伺服器（例如執行 Active Directory Domain Services 的電腦、Active Directory 憑證服務和網路原則伺服器）正在執行 Windows Server 2016。 您可以針對基礎結構伺服器以及執行遠端存取的伺服器，使用舊版的 Windows Server，例如 Windows Server 2012 R2。
>
>請勿嘗試在 Microsoft Azure 的虛擬機器 (VM) 上部署遠端存取。 不支援在 Microsoft Azure 中使用遠端存取，包括遠端存取 VPN 和 DirectAccess。 如需詳細資訊，請參閱[Microsoft Microsoft Azure 虛擬機器的伺服器軟體支援](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。

## <a name="about-this-deployment"></a>關於此部署

提供的指示會引導您使用下列任何案例，針對執行 Windows 10 的遠端用戶端電腦，將遠端存取部署為點對站 VPN 連線的單一租使用者 VPN RAS 閘道。 您也可以找到修改部署的部分現有基礎結構的指示。 此外，在此部署中，您可以找到連結以協助您深入瞭解 VPN 連線程式、要設定的伺服器、ProfileXML VPNv2 CSP 節點，以及部署 Always On VPN 的其他技術。

**Always On VPN 部署案例：**

1. 僅部署 Always On VPN。
2. 使用 Azure AD，透過 VPN 連線的條件式存取來部署 Always On VPN。

如需相關案例的詳細資訊和工作流程，請參閱[部署 ALWAYS ON VPN](always-on-vpn-deploy-deployment.md)。

## <a name="what-isnt-provided-in-this-deployment"></a>此部署中未提供的內容

此部署並未提供下列指示：

- Active Directory Domain Services (AD DS) 。
- Active Directory 憑證服務 (AD CS) ，以及 (PKI) 的公開金鑰基礎結構。
-  (DHCP) 的動態主機設定通訊協定。
- 網路硬體，例如 Ethernet 纜線、防火牆、交換器和集線器。
- 遠端使用者可以透過 Always On VPN 連線存取的其他網路資源，例如應用程式和檔案伺服器。
- 網際網路連線能力，或使用 Azure AD 網際網路連線的條件式存取。 如需詳細資訊，請參閱[Azure Active Directory 中的條件式存取](/azure/active-directory/active-directory-conditional-access-azure-portal)。

## <a name="next-steps"></a>後續步驟

- [深入瞭解 Always On VPN 特性和功能](../../vpn-map-da.md)

- [深入瞭解 Always On VPN 增強功能](../always-on-vpn-enhancements.md)

- [瞭解一些 advanced Always On VPN 功能](always-on-vpn-adv-options.md)

- [深入瞭解 Always On VPN 技術](../always-on-vpn-technology-overview.md)

- [開始規劃您的 Always On VPN 部署](always-on-vpn-deploy-deployment.md)

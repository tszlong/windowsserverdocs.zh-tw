---
title: Windows Server 和 Windows 10 的 Always On VPN 部署
description: 您可以使用此部署適用於 Windows 10 用戶端電腦使用在 Windows Server 2016 或更新版本的遠端存取 」 和 「 一律開啟 」 VPN 設定檔部署的遠端員工永遠在虛擬私人網路 (VPN) 連線。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cb60bdc6d6f3ff074f04827aa95c9e8e8abf35b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859619"
---
# <a name="always-on-vpn-deployment-for-windows-server-and-windows-10"></a>適用於 Windows Server 和 Windows 10 的一律開啟 」 VPN 部署

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows 10

&#171;  [**前一個：** 遠端存取](../../../Remote-Access.md)<br>
&#187;[**下一步:** 深入了解的一律開啟 」 VPN 功能](../../vpn-map-da.md)


一律開啟 」 VPN 提供單一且一致的解決方案，遠端存取 」 和 「 已加入網域的支援、 已加入網域的 （工作群組） 或加入 Azure AD – 裝置，甚至是個人擁有的裝置。  一律開啟 」 VPN，與連線類型不一定要以獨佔方式使用者或裝置，但可以是兩者的組合。 比方說，您可以啟用遠端裝置管理的裝置驗證，然後再啟用使用者驗證連線到內部公司站台及服務。



## <a name="prerequisites"></a>必要條件

您很可能有技術部署，您可以使用部署一律開啟 」 VPN。 您的 DC/DNS 伺服器，非一律開啟 」 VPN 部署需要的 NPS (RADIUS) 伺服器、 憑證授權單位 (CA) 伺服器，以及遠端存取 (路由/VPN) 伺服器。 一旦設定好基礎結構之後，您必須註冊用戶端，並再將用戶端連接到您內部部署安全地透過幾項網路變更。

- Active Directory 網域基礎結構，包括一或多個網域名稱系統 (DNS) 伺服器。 內部和外部網域名稱系統 (DNS) 區域是必要的它會假設內部區域是外部的區域 （例如，corp.contoso.com 和 contoso.com） 的委派子網域。
- Active Directory 型公開金鑰基礎結構 (PKI) 與 Active Directory 憑證服務 (AD CS)。
- 伺服器、 虛擬或實體、 現有或新的若要安裝網路原則伺服器 (NPS)。 如果您已經在網路上 NPS 伺服器，您可以修改現有的 NPS 伺服器設定而非新增新的伺服器。
- 為一小部分的功能，可支援 IKEv2 VPN 連線與區域網路路由與 RAS 閘道 VPN 伺服器的遠端存取。
- 包含兩個防火牆的周邊網路。  請確定您的防火牆允許流量所需的 VPN 和 RADIUS 通訊，才能正確運作。 如需詳細資訊，請參閱 <<c0> [ 一律在 VPN 技術概觀](../always-on-vpn-technology-overview.md)。
- 實體伺服器或與兩個實體的乙太網路介面卡，將遠端存取安裝為 RAS 閘道的 VPN 伺服器周邊網路上的虛擬機器 (VM)。 Vm 主機需要虛擬 LAN (VLAN)。 
- 所需的最小的成員資格系統管理員或同等權限。
- 閱讀本指南，以確保您已準備此部署在執行部署之前的計劃的一節。
- 檢閱每個所使用的技術設計和部署指南。 這些指南可協助您判斷部署案例提供的服務與您組織的網路所需的設定。 如需詳細資訊，請參閱 <<c0> [ 一律在 VPN 技術概觀](../always-on-vpn-technology-overview.md)。
- 您選擇的部署一律開啟 」 VPN 組態，因為 CSP 不是廠商特定的管理平台。


>[!IMPORTANT]
>針對此部署，它不是您的基礎結構伺服器，例如執行 Active Directory 網域服務、 Active Directory 憑證服務，以及網路原則伺服器的電腦執行 Windows Server 2016 的需求。 針對基礎結構伺服器和執行遠端存取伺服器，您可以使用舊版的 Windows Server，例如 Windows Server 2012 R2。
>
>請勿嘗試在虛擬機器上部署遠端存取\(VM\)在 Microsoft Azure 中。 在 Microsoft Azure 中使用遠端存取不支援，包括遠端存取 VPN 和 DirectAccess。 如需詳細資訊，請參閱 < [Microsoft Azure 虛擬機器的 Microsoft 伺服器軟體支援](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。


## <a name="bkmk_about"></a>此部署相關

提供的指示逐步引導您完成將遠端存取部署為單一租用戶 VPN RAS 閘道點\-至\-站台 VPN 連線，使用任何執行 Windows 的遠端用戶端電腦，如下所述的案例10。 您也可以找到修改一些現有的基礎結構部署指示。 也在此部署中，您會發現可協助您深入了解 VPN 連線程序、 要設定的伺服器、 ProfileXML VPNv2 CSP 節點和其他技術來部署一律開啟 」 VPN 連結。

**Alwayson VPN 部署案例：**

1. 只有在 VPN 上一律部署。
2. 使用 VPN 連線使用 Azure AD 條件式存取來部署一律開啟 」 VPN。


如需詳細資訊和工作流程所呈現的案例，請參閱[部署一律開啟 」 VPN](always-on-vpn-deploy-deployment.md)。


## <a name="bkmk_not"></a>此部署中未提供項目

這個部署未提供的指示：

- Active Directory 網域服務\(AD DS\)。
- Active Directory 憑證服務\(AD CS\)和 公開金鑰基礎結構\(PKI\)。
- 動態主機設定通訊協定\(DHCP\)。 
- 網路硬體，例如乙太網路纜線、 防火牆、 交換器以及集線器。
- 其他網路資源，例如應用程式和檔案，遠端使用者可以存取透過一律開啟 」 VPN 連線的伺服器。
- 網際網路連線或網際網路連線，使用 Azure AD 條件式存取。 如需詳細資訊，請參閱 < [Azure Active Directory 中條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。




## <a name="next-steps"></a>後續步驟

- [深入了解的一律開啟 」 VPN 功能](../../vpn-map-da.md)

- [深入了解一律開啟 」 VPN 增強功能](../always-on-vpn-enhancements.md)

- [深入了解一些進階的一律開啟 」 VPN 功能](always-on-vpn-adv-options.md)

- [深入了解一律開啟 」 VPN 技術](../always-on-vpn-technology-overview.md)

- [開始規劃您的一律開啟 」 VPN 部署](always-on-vpn-deploy-deployment.md)


---

---
title: Windows Server 和 Windows 10 的 Always On VPN 部署
description: 您可以使用此部署適用於 windows 10 用戶端電腦使用在 Windows Server 2016 或更新版本的遠端存取和 Always On VPN 設定檔部署為遠端員工一律在虛擬私人網路 (VPN) 連線。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cb60bdc6d6f3ff074f04827aa95c9e8e8abf35b
ms.sourcegitcommit: c435f91ef6f3ff5ffd2661291264b939d5ce4e2a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2018
ms.locfileid: "8981128"
---
# Always On VPN 部署適用於 Windows Server 和 Windows 10

>適用於： Windows Server （半年度管道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #171; [**先前：** 遠端存取](../../../Remote-Access.md)<br>
& #187;[**下一步：** 了解 Always On VPN 功能及功能](../../vpn-map-da.md)


Always On VPN 提供遠端存取和支援已加入網域的單一、 緊密結合解決方案、 未加入網域的 （工作群組） 或加入 Azure AD – 裝置，甚至是個人所擁有的裝置。  Always On VPN，使用連線類型沒有為單獨的使用者或裝置，但可能會將兩者結合。 例如，您可以啟用適用於遠端裝置管理的裝置驗證和啟用為公司內部網站和服務的連線的使用者驗證。



## 必要條件

您最有可能有技術部署，您可以使用部署 Always On VPN。 您的 DC/DNS 伺服器，以外 Always On VPN 部署需要 NPS (RADIUS) 伺服器、 憑證授權單位 (CA) 伺服器和遠端存取 (路由/VPN) 伺服器。 一旦設定好的基礎結構之後，您必須註冊用戶端，然後連線至您內部安全地透過數個網路變更的用戶端。

- Active Directory 網域基礎結構，包括一或多個網域名稱系統 (DNS) 伺服器。 內部和外部網域名稱系統 (DNS) 區域是必要的即假設內部區域是委派子網域的外部的區域 （例如，corp.contoso.com 和 contoso.com）。
- Active Directory 型公開金鑰基礎結構 (PKI) 和 Active Directory 憑證服務 (AD CS)。
- 虛擬或實體、 現有或 server 的新手，安裝網路原則伺服器 (NPS)。 如果您已經在您網路上 NPS 伺服器，您可以修改現有的 NPS 伺服器設定，而不是新增新的伺服器。
- 為支援 IKEv2 VPN 連線和區域網路路由功能的一小部分的 RAS 閘道 VPN 伺服器的遠端存取。
- 適用於周邊網路，其中包含兩個防火牆。  請確定您的防火牆允許流量所需 VPN 和半徑通訊正常運作。 如需詳細資訊，請參閱[一律在 VPN 技術概觀](../always-on-vpn-technology-overview.md)。
- 使用兩個實體的乙太網路介面卡，若要安裝為 RAS 閘道 VPN 伺服器的遠端存取周邊網路上的虛擬機器 (VM) 或實體伺服器。 Vm 主機需要虛擬區域網路 (VLAN)。 
- 在系統管理員或對等項目，會員資格是所需的最小。
- 閱讀此指南，請確定您已準備此部署之前執行部署的規劃一節。
- 檢閱每個使用的技術的設計和部署指南。 下列指南可協助您判斷是否部署案例提供的服務和您需要針對您的組織網路的設定。 如需詳細資訊，請參閱[一律在 VPN 技術概觀](../always-on-vpn-technology-overview.md)。
- 因為雲端解決方案提供者不是廠商特定部署 Always On VPN 設定您選擇的管理平台。


>[!IMPORTANT]
>如需此部署中，這不是假設您的基礎結構伺服器，例如電腦執行 Active Directory 網域服務、 Active Directory 憑證服務及網路原則伺服器，執行 Windows Server 2016 需求。 您可以使用較舊版本的 Windows Server，例如 Windows Server 2012 R2，伺服器所需的基礎結構及執行遠端存取伺服器。
>
>不要嘗試在虛擬機器上部署遠端存取 \(VM\) Microsoft Azure 中的。 在 Microsoft Azure 中使用遠端存取不支援，包括遠端存取 VPN 和 DirectAccess。 如需詳細資訊，請參閱[Microsoft 伺服器軟體支援 Microsoft Azure 的虛擬機器](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。


## <a name="bkmk_about"></a>有關此部署

提供的指示，逐步引導您完成為單一租用戶 VPN RAS 閘道的點數 \ to\ 網站的 VPN 連線，才能使用任何適用於執行 Windows 10 的遠端用戶端電腦，如下所述的案例中部署遠端存取。 您也可以找到適用於修改您現有的基礎結構，適用於部署的一些指示。 也在整個此部署中，您會發現連結可協助您深入了解 VPN 連線處理程序、 伺服器設定、 ProfileXML VPNv2 CSP 節點，以及其他技術，可部署 Always On VPN。

**Always On VPN 部署案例：**

1. 僅在 VPN 一律部署。
2. 部署 Always On VPN 使用的 VPN 連線使用 Azure AD 的條件式存取。


如需詳細資訊和呈現的案例的工作流程，請參閱[部署 Always On VPN](always-on-vpn-deploy-deployment.md)。


## <a name="bkmk_not"></a>什麼是未提供此部署中

此部署不會提供的指示：

- Active Directory 網域服務 \(AD DS\)。
- Active Directory 憑證服務 \(AD CS\) 和公開金鑰基礎結構 \(PKI\)。
- 動態主機設定通訊協定 \(DHCP\)。 
- 網路硬體，例如乙太網路纜線、 防火牆、 切換裝置和中樞。
- 其他網路資源，例如應用程式和檔案，遠端使用者可以存取透過 Always On VPN 連線的伺服器。
- 網際網路連線或使用 Azure AD 的網際網路連線的條件式存取。 如需詳細資訊，請參閱[條件式存取 Azure Active Directory 中](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。




## 後續步驟

- [深入了解 Always On VPN 特色和功能](../../vpn-map-da.md)

- [深入了解 Always On VPN 增強功能](../always-on-vpn-enhancements.md)

- [深入了解的一些進階 Always On VPN 功能](always-on-vpn-adv-options.md)

- [深入了解 Always On VPN 技術](../always-on-vpn-technology-overview.md)

- [開始規劃您的 Always On VPN 部署](always-on-vpn-deploy-deployment.md)


---

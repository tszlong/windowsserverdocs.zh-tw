---
title: 遠端存取 Always On VPN 遷移總覽
description: Always On VPN 可解決 Windows Vpn 和 DirectAccess 之間先前的差距，以及如何從 DirectAccess 遷移至 Always On VPN。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/29/2018
ms.openlocfilehash: d3ea6f0e29803b8a709f31811f77678bf03201a8
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822574"
---
# <a name="overview-of-the-directaccess-to-always-on-vpn-migration"></a>DirectAccess 移轉至 Always On VPN 概觀 

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows 10

&#187;[**下一步：** 規劃 DIRECTACCESS 以 Always On VPN 遷移](da-always-on-migration-planning.md)

在舊版的 Windows VPN 架構中，平臺限制使得您難以提供取代 DirectAccess 所需的重要功能，例如在使用者登入之前起始的自動連線。 不過，Always On VPN 已經減輕了這些限制，或擴充了 DirectAccess 功能以外的 VPN 功能。 Always On VPN 會解決 Windows Vpn 和 DirectAccess 之間先前的差距。

DirectAccess 到 Always On VPN 遷移套裝程式含四個主要元件和高層級的進程：


1.  **規劃 Always On VPN 遷移。** 規劃有助於識別用於使用者階段分隔的目標用戶端，以及基礎結構和功能。

    1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

    2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

    3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

    4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

2.  **部署並存 VPN 基礎結構。** 在決定您的遷移階段和您想要包含在部署中的功能之後，您可以將 Always On VPN 基礎結構與現有 DirectAccess 基礎結構並存部署。  

3.  **將憑證和設定部署到用戶端。**  一旦 VPN 基礎結構準備就緒，您就可以建立所需的憑證，並將其發佈至用戶端。 當用戶端收到憑證時，您會部署 VPN_Profile. ps1 設定腳本。 或者，您可以使用 Intune 來設定 VPN 用戶端。 使用 Microsoft Endpoint Configuration Manager 或 Microsoft Intune 監視是否已成功部署 VPN 設定。

4.  **移除和解除委任。** 在您將所有人從 DirectAccess 遷移之後，將環境適當地解除委任。

    1.  [!INCLUDE [remove-da-from-client-shortdesc-include](../includes/remove-da-from-client-shortdesc-include.md)]

    2.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]


## <a name="directaccess-deployment-scenario"></a>DirectAccess 部署案例

在此部署案例中，您會使用簡單的 DirectAccess 部署案例，做為此指南所提供的遷移起點。 在遷移至 Always On VPN 之前，您不需要符合此部署案例，但對於許多組織而言，這項簡單的安裝程式是其目前 DirectAccess 部署的精確表示。 下表提供此設定的基本功能清單。

許多 DirectAccess 部署案例和選項都存在，因此您的執行可能會與此處所述的不同。 若是如此，請參閱[DirectAccess 與 ALWAYS ON VPN 之間的功能對應](../vpn/vpn-map-da.md)，以判斷目前新增專案的 Always On VPN 功能集對應，然後將這些功能新增至您的設定。 此外，您可以參閱[ALWAYS ON vpn 增強功能](../vpn/always-on-vpn/always-on-vpn-enhancements.md)，以將選項新增至您的 Always On VPN 部署。

>[!NOTE] 
>針對未加入網域的裝置，還有其他考慮，例如憑證註冊。 如需詳細資訊，請參閱[適用于 Windows Server 和 windows 10 的 ALWAYS ON VPN 部署](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)。

### <a name="deployment-scenario-feature-list"></a>部署案例功能清單

| DirectAccess 功能 | 一般案例 |
|-----|----|
| 部署案例                   | 部署完整 DirectAccess 以進行用戶端存取和遠端系統管理                                               |
| 網路介面卡                      | 2                                                                                                              |
| 使用者驗證                   | Active Directory 認證                                                                                   |
| 使用電腦憑證             | [是]                                                                                                            |
| 安全性群組                       | [是]                                                                                                            |
| 單一 DirectAccess 伺服器            | [是]                                                                                                            |
| 網路拓撲                      | 具有兩張網路介面卡的邊緣防火牆後方的網路位址轉譯（NAT）                            |
| 存取模式                           | 端對邊緣                                                                                                    |
| 通道                             | 分割通道                                                                                                   |
| Authentication                        | 使用電腦憑證加上 Kerberos 的標準公開金鑰基礎結構（PKI）驗證（非 KerbProxy） |
| 通訊協定                             | 透過 HTTPS 的 IP （ip-HTTPs）                                                                                       |
| 網路位置伺服器（NLS）已關閉-box | [是]                                                                                                            |

## <a name="always-on-vpn-deployment-scenario"></a>Always On VPN 部署案例

在此部署案例中，您將焦點放在將簡單的 DirectAccess 環境遷移至簡單的 Always On VPN 環境，也就是 DirectAccess 取代解決方案。 下表提供此簡單方案中使用的功能。 如需有關 Always On VPN 用戶端其他增強功能的詳細資訊，請參閱[ALWAYS ON VPN 增強功能](../vpn/always-on-vpn/always-on-vpn-enhancements.md)。

### <a name="always-on-vpn-features-used-in-the-simple-environment"></a>在簡單的環境中使用 Always On VPN 功能

| VPN 功能 | 部署案例設定 |
|-----|-----|
| 連線類型 | Native 網際網路金鑰交換版本2（IKEv2） |
| 網路介面卡   | 2        |
| 使用者驗證  | Active Directory 認證            |
| 使用電腦憑證        | [是]                          |
| 路由 | 分割通道 |
| 名稱解析 | 功能變數名稱資訊清單和網域名稱系統（DNS）尾碼 |
| 各個 | Always on 和信任的網路偵測 |
| Authentication  | 受保護的可延伸驗證通訊協定-傳輸層安全性（PEAP-TLS）與可信賴平臺模組-受保護的使用者憑證 |

## <a name="next-step"></a>後續步驟

[規劃 DirectAccess 以 ALWAYS ON VPN 遷移](da-always-on-migration-planning.md)。 遷移的主要目的是讓使用者在整個過程中維持對辦公室的遠端連線。

---
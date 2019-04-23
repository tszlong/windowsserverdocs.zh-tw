---
title: 虛擬私人網路 (VPN)
description: 您可以使用本主題來了解 Windows Server 2016 和 Windows 10 VPN 功能和功能。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: bf38995f0a2b396044d1f45b45eff8c3c2de329d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891069"
---
# <a name="virtual-private-networking-vpn"></a>虛擬私人網路\(VPN\)

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows 10

## <a name="ras-gateway-as-a-single-tenant-vpn-server"></a>做為單一租用戶的 VPN 伺服器的 RAS 閘道

在 Windows Server 2016 中，遠端存取伺服器角色是下列相關的網路存取技術的邏輯群組。

- 遠端存取服務 (RAS)
- 路由
- Web 應用程式 Proxy

這些技術是遠端存取伺服器角色的角色服務。

當您使用新增角色及功能精靈 或 Windows PowerShell 安裝遠端存取伺服器角色時，您可以安裝一或多個這些三個角色服務。

當您安裝**DirectAccess 和 VPN (RAS)** 角色服務，您要部署 「 遠端存取服務通訊閘\( **RAS 閘道**\)。 您可以部署 RAS 閘道作為單一租用戶 RAS 閘道虛擬私人網路\(VPN\)伺服器，可提供許多進階功能和增強功能。

>[!NOTE]
>您也可以為多租用戶的 VPN 伺服器用於軟體定義網路部署 RAS 閘道\(SDN\)，或做為 DirectAccess 伺服器。 如需詳細資訊，請參閱 < [RAS 閘道](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway)，[軟體定義網路 (SDN)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)，並[DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess)。

## <a name="related-topics"></a>相關主題
- [一律開啟 」 VPN 特性與功能](vpn-map-da.md):本主題中，您會了解的一律開啟 」 VPN 的功能。 

- [在 Windows 10 中設定 VPN 裝置通道](vpn-device-tunnel-config.md):一律開啟 」 VPN 可讓您能夠建立專用的 VPN 設定檔針對裝置或電腦。 一律開啟 」 VPN 連線中包含兩種通道類型：_裝置通道_並_使用者通道_。 裝置通道用於進行前的登入的連接狀況和裝置管理。 使用者通道可讓使用者透過 VPN 伺服器存取組織資源。

- [Always On Windows Server 2016 和 Windows 10 VPN 部署](always-on-vpn/deploy/always-on-vpn-deploy.md):提供有關部署 「 遠端存取的單一租用戶 VPN RAS 閘道，點\-至\-站台 VPN 連線可讓您連接到您的組織網路，其一律開啟 」 VPN 連線的遠端員工。 建議您檢閱每個此部署所使用的技術設計和部署指南。

- [Windows 10 VPN 技術指南](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide):引導您完成將您的 VPN 解決方案的企業中的 Windows 10 用戶端以及如何設定您的部署決策。 您可以尋找參考至 VPNv2 組態服務提供者 (CSP)，並提供行動裝置管理 (MDM) 設定的指示適用於 Windows 10 中使用 Microsoft Intune 和 VPN 設定檔範本。

- [如何建立 VPN 設定檔在 System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles):本主題中，您將了解如何建立 VPN 設定檔在 System Center Configuration Manager (SCCM)。

- [設定 Windows 10 用戶端一律開啟 VPN 連線](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections):本主題描述 ProfileXML 選項和結構描述，以及如何建立 ProfileXML VPN。 設定好的伺服器基礎結構之後，您必須設定 Windows 10 用戶端電腦通訊的 VPN 連線與該基礎結構。 

- [VPN 設定檔選項](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options):本主題說明在 Windows 10 VPN 設定檔設定，並了解如何設定 VPN 設定檔使用 Intune 或 SCCM。 您可以使用 ProfileXML 節點 VPNv2 CSP 中的 Windows 10 中設定所有的 VPN 設定。

---
---
title: 虛擬私人網路 (VPN)
description: 您可以使用本主題來瞭解 Windows Server 2016 和 Windows 10 VPN 的功能。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: lizross
author: eross-msft
ms.localizationpriority: medium
ms.openlocfilehash: 55e99d4a02b88a7b5367d3b389237bf96829fd89
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80307800"
---
# <a name="virtual-private-networking-vpn"></a>虛擬私人網路 (VPN)

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows 10

## <a name="ras-gateway-as-a-single-tenant-vpn-server"></a>作為單一租使用者 VPN 伺服器的 RAS 閘道

在 Windows Server 2016 中，遠端存取服務器角色是下列相關網路存取技術的邏輯群組。

- 遠端存取服務（RAS）
- 路由
- Web 應用程式 Proxy

這些技術是遠端存取服務器角色的角色服務。

當您使用 [新增角色及功能] Wizard 或 Windows PowerShell 安裝遠端存取服務器角色時，您可以安裝這三個角色服務中的一或多個。

當您安裝**DirectAccess 和 VPN （ras）** 角色服務時，您會部署遠端存取服務閘道（**ras 閘道**）。 您可以將 RAS 閘道部署為單一租使用者 RAS 閘道虛擬私人網路（VPN）伺服器，以提供許多先進的功能和增強功能。

>[!NOTE]
>您也可以將 RAS 閘道部署為多租使用者 VPN 伺服器，以與軟體定義網路（SDN）或 DirectAccess 伺服器搭配使用。 如需詳細資訊，請參閱[RAS 閘道](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway)、[軟體定義網路功能（SDN）](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)和[DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess)。

## <a name="related-topics"></a>相關主題
- [ALWAYS ON vpn 特性和功能](vpn-map-da.md)：在本主題中，您將瞭解 Always On VPN 的特性和功能。 

- [在 Windows 10 中設定 Vpn 裝置通道](vpn-device-tunnel-config.md)： Always On VPN 可讓您為裝置或電腦建立專用的 vpn 設定檔。 Always On VPN 連接包含兩種類型的通道：_裝置_通道和_使用者_通道。 裝置通道用於登入連線案例和裝置管理用途。 使用者通道可讓使用者透過 VPN 伺服器存取組織資源。

- [適用于 Windows Server 2016 和 windows 10 的 ALWAYS ON VPN 部署](always-on-vpn/deploy/always-on-vpn-deploy.md)：提供將遠端存取部署為點對站 vpn 連線的單一租使用者 Vpn RAS 閘道的指示，讓您的遠端員工能夠 Always On VPN 連線連接到您的組織網路。 建議您參閱設計和部署指南，以瞭解此部署中所使用的每項技術。

- [Windows 10 VPN 技術指南](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide)：引導您完成企業 VPN 解決方案中的 Windows 10 用戶端所做的決定，以及如何設定您的部署。 您可以找到 VPNv2 設定服務提供者（CSP）的參考，並使用 Microsoft Intune 和適用于 Windows 10 的 VPN 設定檔範本來提供行動裝置管理（MDM）設定指示。

- [如何在 Configuration Manager 中建立 vpn 設定檔](https://docs.microsoft.com/configmgr/protect/deploy-use/create-vpn-profiles)：在本主題中，您將瞭解如何在 Configuration Manager 中建立 vpn 設定檔。

- [設定 Windows 10 用戶端 ALWAYS ON VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections)連線：本主題說明 ProfileXML 選項和架構，以及如何建立 ProfileXML VPN。 設定伺服器基礎結構之後，您必須將 Windows 10 用戶端電腦設定為使用 VPN 連線與該基礎結構進行通訊。

- [Vpn 設定檔選項](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)：本主題說明 Windows 10 中的 vpn 設定檔設定，並瞭解如何使用 Intune 或 Configuration Manager 來設定 vpn 設定檔。 您可以使用 VPNv2 CSP 中的 ProfileXML 節點來設定 Windows 10 中的所有 VPN 設定。

---
title: 虛擬私人網路 (VPN)
description: 您可以使用本主題以了解 Windows Server 2016 和 Windows 10 VPN 特色和功能。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: bf38995f0a2b396044d1f45b45eff8c3c2de329d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067121"
---
# 虛擬私人網路 \(VPN\)

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows 10

## RAS 閘道，為單一租用戶 VPN 伺服器

在 Windows Server 2016 中，遠端存取伺服器角色是下列相關的網路存取技術的邏輯群組。

- 遠端存取服務 (RAS)
- 路由
- Web 應用程式 Proxy

這些技術是遠端存取伺服器角色的角色服務。

當您新增角色及功能精靈或 Windows PowerShell 安裝遠端存取伺服器角色時，您可以安裝一或多個這些三個角色服務。

當您安裝**DirectAccess 和 VPN (RA)** 角色服務時，您要部署遠端存取服務閘道 \ (**RAS 閘道**\)。 您可以將 RAS 閘道部署為單一租用戶 RAS 閘道虛擬私人網路 \(VPN\) 伺服器，提供許多進階功能和增強功能。

>[!NOTE]
>您也可以部署 RAS 閘道，為多組織用戶 VPN 伺服器以搭配軟體定義網路 \(SDN\)，或做為 DirectAccess 伺服器。 如需詳細資訊，請參閱[RAS 閘道](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway)、[軟體定義網路 (SDN)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)，以及[DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess)。

## 相關主題
- [Always On VPN 特色和功能](vpn-map-da.md)： 在本主題中，您了解的 Always On VPN 的功能。 

- [在 Windows 10 中設定 VPN 裝置通道](vpn-device-tunnel-config.md)： Always On VPN 可讓您能夠建立專用的 VPN 設定檔的裝置或電腦。 Always On VPN 連線包括兩種類型的通道：_裝置通道_和_使用者通道_。 使用裝置通道預先登入的連線案例和裝置管理目的。 使用者通道可讓使用者透過 VPN 伺服器存取組織資源。

- [一律在 VPN 部署適用於 Windows Server 2016 和 Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md)： 提供指示上部署遠端存取做為單一租用戶允許遠端連線到您的組織員工的點數 \ to\ 網站 VPN 連線的 VPN RAS 閘道Always On VPN 連線的網路。 建議您檢閱每個此部署中使用的技術的設計和部署指南。

- [Windows 10 VPN 技術指南](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide)： 將引導您完成您的企業 VPN 解決方案，以及如何設定您的部署中進行的 Windows 10 用戶端的決策。 您可以找出至 VPNv2 設定服務提供者 (CSP) 的參考，並提供行動裝置管理 (MDM) 設定指示適用於 Windows 10 中使用 Microsoft Intune 和 VPN 設定檔 」 範本。

- [如何建立 VPN 設定檔中 System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles)： 在本主題中，您了解如何在 System Center Configuration Manager (SCCM) 建立 VPN 設定檔。

- [設定 Windows 10 用戶端一律在 VPN 連線](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections)： 本主題描述 ProfileXML 選項和結構描述，以及如何建立 ProfileXML VPN。 設定好伺服器基礎結構之後，您必須設定 VPN 連線使用該基礎結構與通訊的 windows 10 用戶端電腦。 

- [VPN 設定檔選項](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)： 本主題描述在 Windows 10 中的 VPN 設定檔設定，並了解如何使用 Intune 或 SCCM VPN 設定檔設定。 您可以在使用 ProfileXML 節點 VPNv2 CSP 中的 Windows 10 中設定所有 VPN 設定。

---
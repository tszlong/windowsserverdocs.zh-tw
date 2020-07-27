---
title: 租用戶內部部署元件
description: 描述您 RDS 部署中的內部部署元件。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.topic: article
ms.assetid: b3eebb38-a835-4fa6-9e41-1966014bf2cb
author: lizap
manager: dongill
ms.openlocfilehash: 3c29eb20c4cf39c868bab07fdfddc50aec5c4b85
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86953491"
---
# <a name="tenant-on-premises-components"></a>租用戶內部部署元件

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

下列資訊描述構成桌面主機部署的內部部署元件。  
  
##  <a name="clients"></a>用戶端  
若要存取裝載的桌面和應用程式，使用者必須使用支援遠端桌面通訊協定 (RDP) 7.1 或更新版本的遠端桌面用戶端。 尤其，用戶端必須支援遠端桌面閘道和遠端桌面連線代理人。 若要將應用程式傳遞到本機桌面，用戶端也必須支援 RemoteApp 功能。 若要達到最大的閘道規模，用戶端必須支援對 RD 閘道的純 HTTP 傳輸連線。  
  
其他資訊：  
[Windows Server 2012 R2 遠端桌面閘道的新功能](https://techcommunity.microsoft.com/t5/microsoft-security-and/what-8217-s-new-in-windows-server-2012-remote-desktop-gateway/ba-p/247611)  
[Microsoft 遠端桌面用戶端](./clients/remote-desktop-clients.md)  
[Microsoft Store 中的 Windows 遠端桌面應用程式](https://apps.microsoft.com/windows/app/remote-desktop/051f560e-5e9b-4dad-8b2e-fa5e0b05a480)  
[Microsoft 遠端桌面 - Google Play 的 Android 應用程式](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android)  
[Mac App Store - Microsoft 遠端桌面](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417?mt=12)  
[App Store 中的 Microsoft 遠端桌面](https://itunes.apple.com/app/microsoft-remote-desktop/id714464092?mt=8)  
  
##  <a name="active-directory-domain-services"></a>Active Directory 網域服務  
某些較大且較複雜的租用戶可以選擇在內部裝載 Active Directory 網域服務 (AD DS) 伺服器。 在此情況下，租用戶環境中的 AD DS 伺服器，通常會是租用戶內部部署 AD DS 伺服器的複本。 在租用戶環境中建立虛擬網路，並使用 Azure VPN 建立從租用戶內部部署網路到位於 Azure 資料中心租用戶虛擬網路的站對站連線，即可支援這一點。  
  
其他資訊：  
[Microsoft Azure 虛擬網路概觀](/azure/virtual-network/virtual-networks-overview)  
[使用 Azure 入口網站建立具有站對站 VPN 連線的資源管理員 VNet](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)  

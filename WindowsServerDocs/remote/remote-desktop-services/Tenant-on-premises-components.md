---
title: 租用戶內部部署元件
description: 描述您 RDS 部署中的內部部署元件。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b3eebb38-a835-4fa6-9e41-1966014bf2cb
author: lizap
manager: dongill
ms.openlocfilehash: a9ff1382d2a2e7e2acf0247fa2ba4ae8e9642162
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387838"
---
# <a name="tenant-on-premises-components"></a>租用戶內部部署元件

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

下列資訊描述構成桌面主機部署的內部部署元件。  
  
##  <a name="clients"></a>用戶端  
若要存取裝載的桌面和應用程式，使用者必須使用支援遠端桌面通訊協定 (RDP) 7.1 或更新版本的遠端桌面用戶端。 尤其，用戶端必須支援遠端桌面閘道和遠端桌面連線代理人。 若要將應用程式傳遞到本機桌面，用戶端也必須支援 RemoteApp 功能。 若要達到最大的閘道規模，用戶端必須支援對 RD 閘道的純 HTTP 傳輸連線。  
  
其他資訊：  
[已啟用 RemoteFX 的裝置](https://social.technet.microsoft.com/wiki/contents/articles/14534.remotefx-enabled-devices.aspx)  
[Windows Server 2012 R2 遠端桌面閘道的新功能](https://blogs.technet.microsoft.com/enterprisemobility/2013/03/14/whats-new-in-windows-server-2012-remote-desktop-gateway/#transport)  
[Microsoft 遠端桌面用戶端](https://technet.microsoft.com/library/dn473009.aspx)  
[Microsoft Store 中的 Windows 遠端桌面應用程式](https://apps.microsoft.com/windows/app/remote-desktop/051f560e-5e9b-4dad-8b2e-fa5e0b05a480)  
[Microsoft 遠端桌面 - Google Play 的 Android 應用程式](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android)  
[Mac App Store - Microsoft 遠端桌面](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417?mt=12)  
[App Store 中的 Microsoft 遠端桌面](https://itunes.apple.com/app/microsoft-remote-desktop/id714464092?mt=8)  
  
##  <a name="active-directory-domain-services"></a>Active Directory Domain Services  
某些較大且較複雜的租用戶可以選擇在內部裝載 Active Directory 網域服務 (AD DS) 伺服器。 在此情況下，租用戶環境中的 AD DS 伺服器，通常會是租用戶內部部署 AD DS 伺服器的複本。 在租用戶環境中建立虛擬網路，並使用 Azure VPN 建立從租用戶內部部署網路到位於 Azure 資料中心租用戶虛擬網路的站對站連線，即可支援這一點。  
  
其他資訊：  
[Microsoft Azure 虛擬網路概觀](https://azure.microsoft.com/documentation/articles/virtual-networks-overview/)  
[使用 Azure 入口網站建立具有站對站 VPN 連線的資源管理員 VNet](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  



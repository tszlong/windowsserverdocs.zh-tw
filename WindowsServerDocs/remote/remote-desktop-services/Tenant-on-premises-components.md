---
title: 租用戶內部部署元件
description: 描述在 RDS 部署中的內部部署元件。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a01dbd12d76b1efa84e38f2ded38cfd613fb2ac4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857399"
---
# <a name="tenant-on-premises-components"></a>租用戶內部部署元件

>適用於：Windows Server （半年通道），Windows Server 2016

下列資訊描述構成桌面主機部署在內部部署元件。  
  
##  <a name="clients"></a>用戶端  
若要存取託管的桌面和應用程式，使用者必須使用支援遠端桌面通訊協定 (RDP) 7.1 或更新版本的遠端桌面用戶端。 特別是，用戶端必須支援遠端桌面閘道和遠端桌面連線代理人。 若要傳遞到本機桌面應用程式，用戶端也必須支援的 RemoteApp 功能。 若要達到最大的閘道規模，用戶端必須支援 RD 閘道的純 HTTP 傳輸連線。  
  
其他資訊：  
[已啟用 RemoteFX 的裝置](https://social.technet.microsoft.com/wiki/contents/articles/14534.remotefx-enabled-devices.aspx)  
[什麼是 Windows Server 2012 R2 遠端桌面閘道的新功能](https://blogs.technet.microsoft.com/enterprisemobility/2013/03/14/whats-new-in-windows-server-2012-remote-desktop-gateway/#transport)  
[Microsoft 遠端桌面用戶端](https://technet.microsoft.com/library/dn473009.aspx)  
[在 Microsoft Store 中的 Windows 遠端桌面應用程式](https://apps.microsoft.com/windows/app/remote-desktop/051f560e-5e9b-4dad-8b2e-fa5e0b05a480)  
[Microsoft 遠端桌面-Google play 的 Android 應用程式](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android)  
[Mac App Store 的 Microsoft 遠端桌面](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12)  
[App Store 中的 Microsoft 遠端桌面](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8)  
  
##  <a name="active-directory-domain-services"></a>Active Directory Domain Services  
某些較大且更精確的租用戶可以選擇裝載在其內部部署 Active Directory 網域服務 (AD DS) 伺服器。 在此情況下，在租用戶的環境中的 AD DS 伺服器通常會是未在租用戶的內部部署的 AD DS 伺服器的複本。 支援此功能，在租用戶的環境中建立虛擬網路，並使用從租用戶的內部部署網路的站對站連線建立至 Azure 資料中心中的租用戶的虛擬網路的 Azure VPN。  
  
其他資訊：  
[Microsoft Azure 虛擬網路概觀](https://azure.microsoft.com/documentation/articles/virtual-networks-overview/)  
[具有站對站 VPN 連線使用 Azure 入口網站建立資源管理員 VNet](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  



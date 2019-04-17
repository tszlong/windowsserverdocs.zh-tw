---
title: "Windows Server Essentials 安裝之前"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8d0893bd-e2b7-4494-9537-02b1cbbcd57a
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e7eb1b7bed780b41f1a87589add4ab015f41624a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="before-you-install-windows-server-essentials"></a>Windows Server Essentials 安裝之前

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

##  <a name="BKMK_BeforeYouBegin"></a>您的 Windows Server Essentials 安裝在您開始之前，請執行下列工作：  

-   **確定您的電腦符合最低的硬體需求**。 這包括判斷是否您需要額外的硬體及驗證您的硬體驅動程式，Windows Server Essentials 的支援。 如需詳細資訊，請查看[系統需求適用於 Windows Server Essentials](../get-started/system-requirements.md)。   

  
    > [!IMPORTANT]
    >  現有的電腦上安裝 Windows Server Essentials 之前，我們建議您完全格式，然後重新預先存在電腦的硬碟磁碟分割。 格式化重新硬碟磁碟分割，您會移除所有的磁碟分割停留在硬碟的可能性。  
  
-   **準備您的網路**若要準備您要安裝 Windows Server Essentials 的網路，執行下列動作：  
    
  
    -   **先升級作業系統 client 電腦上**Windows Server Essentials 支援下列作業系統： Windows 8、 Windows 7、 Windows 10 和 Macintosh 作業系統 X 開普敦或更高。 這些作業系統提供所需的安全性功能、 可靠性，效能和的區域網路功能。  
  
    -   **設定您的路由器**確認您的路由器設定方式如下：  
  
        -   在您的路由器被支援 UPnP 架構。  
  
        -   可以將支援動態主機設定通訊協定 」 (DHCP) 伺服器服務的區域網路或停用。  Windows Server Essentials 確保 DHCP 不伺服器和路由器上執行？ 時 DHCP 在路由器支援，DHCP 會不支援伺服器上，在安裝期間。  
  
        -   您可以為您的路由器，提供網際網路服務提供者 (ISP) 的外部介面 IP 位址。 IP 位址可以在您的 ISP 服務 DHCP 伺服器動態指派，或是您必須手動設定使用路由器管理主控台靜態 IP 位址。  
  
        -   如果您的網際網路連接要求的使用者名稱和密碼，也透過乙太網路 (PPPoE)，稱為點對點通訊協定這些設定的設定在您的路由器，即使裝置支援 UPnP 架構。  
  
        -   路由器已連接到區域網路和網際網路、 已，以及正常運作。  
  
     如果您的路由器不支援 UPnP 架構，或如果您不能設定您的路由器，在安裝期間，您必須手動設定使用您的網路設定。 確定下列連接埠開放的並導向之目的伺服器的 IP 位址：  
  
    |連接埠號碼|應用程式|  
    |-----------------|-----------------|  
    |連接埠 80|HTTP 網路流量|  
    |連接埠 443|HTTPS 網路流量|  
  

-   **朗讀文件的 Windows Server Essentials 發行**。 版本的文件包含最新的適當地安裝並設定 Windows Server Essentials 的重要資訊。 若要檢視或列印發行的文件，請查看[發行的文件適用於 Windows Server Essentials](../get-started/release-notes.md)。  
  
## <a name="see-also"></a>也了  
  
-   [安裝 Windows Server Essentials](Install-Windows-Server-Essentials.md)


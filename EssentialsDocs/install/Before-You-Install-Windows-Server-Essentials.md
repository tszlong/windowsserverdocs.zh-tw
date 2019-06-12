---
title: 在您安裝 Windows Server Essentials 之前
description: 描述如何使用 Windows Server Essentials
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
ms.openlocfilehash: 32b2b37e0d0109b8ad2a991b9f7693139103734d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433717"
---
# <a name="before-you-install-windows-server-essentials"></a>在您安裝 Windows Server Essentials 之前

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_BeforeYouBegin"></a> 在開始安裝的 Windows Server Essentials 之前，請執行下列工作：  

-   **確認您的電腦符合最低硬體需求**。 這包括確認您是否需要額外的硬體，並確認您的硬體的驅動程式會受到 Windows Server Essentials。 如需詳細資訊，請參閱 <<c0> [ 系統需求 Windows Server Essentials 的](../get-started/system-requirements.md)。   


~~~
> [!IMPORTANT]
>  Before you install  Windows Server Essentials on a pre-existing computer, we recommend that you fully format and then repartition the hard disks of the pre-existing computer. By formatting and repartitioning the hard disks, you remove the possibility that hidden partitions remain on the hard disks.  
~~~

- **準備您的網路**若要準備網路以安裝 Windows Server Essentials，執行下列動作：  


  - **您的用戶端電腦上的作業系統升級**Windows Server Essentials 支援下列作業系統：Windows 8、 Windows 7、 Windows 10 和 Macintosh OS X Lion 或更新版本。 這些作業系統可提供區域網路必要的安全功能、可靠性效能以及功能。  

  - **設定您的路由器** 請確認您的路由器已按照下面的方式設定：  

    -   已在路由器上啟用 UPnP 架構。  

    -   可以啟用或停用 LAN 的動態主機設定通訊協定 (DHCP) 伺服器服務。  Windows Server Essentials 可確保 DHCP 不會在伺服器與路由器上執行？ 在路由器上啟用 DHCP 時，DHCP 是不在伺服器上安裝期間啟用。  

    -   您擁有由網際網路服務提供者 (ISP) 提供的路由器外部介面的 IP 位址。 IP 位址可由位在 ISP 的 DHCP 伺服器服務動態指派，否則您就需要使用路由器管理主控台手動設定靜態 IP 位址。  

    -   如果您的網際網路連線要求使用者名稱與密碼 (也稱為乙太網路上的點對點通訊協定 (PPPoE))，即使裝置支援 UPnP 架構，這些設定也都會配置在您的路由器上。  

    -   路由器已連線至 LAN 以及網路網路，已經開啟並且正常作用。  

    如果您的路由器不支援 UPnP 架構，或者無法在安裝期間設定路由器，您就必須以您的網路設定手動配置。 請確定下列連接埠皆已開啟並被導向至目的地伺服器的 IP 位址：  

  |連接埠號碼|應用程式|  
  |-----------------|-----------------|  
  |連接埠 80|HTTP 網路流量|  
  |連接埠 443|HTTPS 網路流量|  


- **閱讀 Windows Server Essentials 版本文件**。 版本文件包含最新的資訊可能相當重要，正確地安裝和設定 Windows Server Essentials。 若要檢視或列印版本文件，請參閱[Release Documentation for Windows Server Essentials](../get-started/release-notes.md)。  

## <a name="see-also"></a>另請參閱  

-   [安裝 Windows Server Essentials](Install-Windows-Server-Essentials.md)


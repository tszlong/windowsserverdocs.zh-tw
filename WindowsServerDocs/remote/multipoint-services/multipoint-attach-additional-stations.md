---
title: 將其他站台附加至您的 MultiPoint 伺服器
description: 將多個站台新增至您的 MultiPoint 服務部署
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d78ebf4e-0968-4014-9a42-9f75cc50cb52
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 57fc8ed6774c3266298ecd98e8f609ec01f63ef6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889259"
---
# <a name="attach-additional-stations-to-multipoint-services"></a>將其他站台連接到 MultiPoint 服務
在 MultiPoint 服務環境中，您的使用者會使用站台連接到 MultiPoint 服務並執行其工作。 站台是連線到執行 Multipoint 服務電腦的使用者端點。  
  
MultiPoint 服務支援三種類型的站台：  
  
-   直接視訊連線的站台  
  
-   USB 零用戶端連線站台 （和 USB 乙太網路零連線用戶端站台移轉）  
  
-   RDP over LAN 連線的工作站  
  
分類根據站台的硬體和它所使用的連線類型而定。 您可以混合使用並符合您的站台的連線類型。 唯一的需求是主要站台 （其中您稍早安裝） 必須直接視訊連線的站台。 如需有關站台設定的詳細資訊，請參閱[MultiPoint 站台](MultiPoint-services-Stations.md)。  
  
說明如何設定每一種站台的指示，請參閱下列各項：  
  
-   [設定要直接視訊連線的站台](Set-up-a-direct-video-connected-station-in-MultiPoint-services.md)  
  
-   [設定 USB 零個用戶端連線的站台](Set-up-a-USB-zero-client-connected-station-in-MultiPoint-services.md)  
  
-   [設定 RDP 移轉網路連接站台](Set-up-an-RDP-over-LAN-connected-station-in-MultiPoint-services.md)  
  
如需站台類型的詳細比較，請參閱[站台類型比較](multipoint-services-stations.md#BKMK_StationTypeComparison)。  
  
> [!NOTE]  
> -   附加站台的程序不會說明如何設定中繼集線器或下游集線器。 如需在何處安裝這些中樞的資訊，請參閱[MultiPoint 站台](MultiPoint-services-Stations.md)。  
> -   在某些情況下，您可能需要在虛擬機器中執行的虛擬桌面，建立站台。 例如，您使用無法在 Windows Server 安裝的應用程式或將不在相同的主機電腦執行多個執行個體的應用程式。 如需詳細資訊，請參閱 <<c0> [ 建立的 Windows 10 企業版虛擬桌面站台的](Create-Windows-10-Enterprise-virtual-desktops-for-stations.md)。  
  
> [!TIP]  
> 很有用，如此就會以循序方式識別 MultiPoint Server 中建立您的站台，其實體位置順序。 如果您稍後想要變更站台的名稱，可在 MultiPoint 管理員，以進行您的選擇。 如需詳細資訊，請參閱重新對應所有廣播站 MultiPoint Server 說明及支援 中。
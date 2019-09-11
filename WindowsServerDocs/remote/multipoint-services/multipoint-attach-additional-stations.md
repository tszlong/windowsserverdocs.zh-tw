---
title: 將其他工作站連接到 MultiPoint 伺服器
description: 將更多工作站新增至 MultiPoint 服務部署
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
ms.openlocfilehash: 70609d491f5eb60daf89df219c06c8b9d4c3cd3e
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871409"
---
# <a name="attach-additional-stations-to-multipoint-services"></a>將其他工作站附加至 MultiPoint 服務
在 MultiPoint 服務環境中，您的使用者會使用工作站來連線到 MultiPoint 服務，並執行其工作。 這些工作站是用來連接到執行 Multipoint 服務之電腦的使用者端點。  
  
MultiPoint 服務支援三種類型的工作站：  
  
-   直接連接視頻的工作站  
  
-   USB 零用戶端連線的工作站（以及透過乙太網路的 USB 零用戶端連線的工作站）  
  
-   RDP over LAN 連線的工作站  
  
分類是以工作站的硬體和其使用的連線類型為基礎。 您可以混合和比對您工作站的連線類型。 唯一的要求是主要工作站（您先前安裝的）必須是直接連線到視頻的工作站。 如需工作站設置的詳細資訊，請參閱[MultiPoint 電臺](MultiPoint-services-Stations.md)。  
  
如需說明如何設定每部工作站類型的指示，請參閱下列各項：  
  
-   [設定直接視訊連線站台](Set-up-a-direct-video-connected-station-in-MultiPoint-services.md)  
  
-   [設定 USB 極簡型用戶端連線站台](Set-up-a-USB-zero-client-connected-station-in-MultiPoint-services.md)  
  
-   [設定 RDP-over-LAN 連線站台](Set-up-an-RDP-over-LAN-connected-station-in-MultiPoint-services.md)  
  
如需工作站類型的詳細比較，請參閱[工作站類型比較](multipoint-services-stations.md#BKMK_StationTypeComparison)。  
  
> [!NOTE]  
> -   連接工作站的程式不會說明如何設定中繼中樞或下游中樞。 如需有關安裝這些中樞之位置的詳細資訊，請參閱[MultiPoint 電臺](MultiPoint-services-Stations.md)。  
> -   在某些情況下，您可能需要建立在虛擬機器中執行的工作站虛擬桌面電腦。 例如，您可以使用無法安裝在 Windows Server 上的應用程式，或不會在同一部主機電腦上執行多個實例的應用程式。 如需詳細資訊，請參閱[建立適用于工作站的 Windows 10 企業版虛擬桌面](Create-Windows-10-Enterprise-virtual-desktops-for-stations.md)。  
  
> [!TIP]  
> 以實體位置的順序建立您的工作站會很有説明，以便在 MultiPoint Server 中依序識別它們。 如果您稍後想要變更工作站的名稱，您可以在 [MultiPoint 管理員] 中執行此動作。 如需詳細資訊，請參閱在 MultiPoint Server 說明和支援中重新對應所有工作站。
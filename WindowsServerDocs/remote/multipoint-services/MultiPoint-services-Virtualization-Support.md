---
title: MultiPoint 服務虛擬化支援
description: 描述如何使用 HYPER-V 的 MultiPoint 服務
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f0864b8-a087-4890-94ef-05efbd3c4241
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 06d518dcea154ac2bab49a7d0e83a90f96be6e44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872519"
---
# <a name="multipoint-services-virtualization-support"></a>MultiPoint 服務虛擬化支援
MultiPoint 服務會支援 HYPER-V 角色，有兩種：  
  
-   MultiPoint 服務可以部署為執行 HYPER-V 的伺服器上的客體作業系統。  
  
-   MultiPoint 服務可用來當做虛擬化伺服器。   
  
在虛擬機器上執行 MultiPoint 服務提供 HYPER-V 工具來管理作業系統使用。 這些工具包括檢查點和復原功能，並可讓您匯出和匯入虛擬機器。 對於較大的安裝，您都可以在單一實體伺服器上執行多個 MultiPoint 服務的虛擬電腦來彙總伺服器。 可能的案例包括：  
  
-   教室或實驗室有超過 20 個基座。 而不是部署多部執行 MultiPoint 服務的實體電腦，您可以部署單一的實體電腦上的多部虛擬機器。  
  
    > [!NOTE]  
    > 無論是實體或虛擬的透過單一的 MultiPoint 管理員主控台，您可以管理多部 MultiPoint 伺服器。  
  
-   MultiPoint server 的虛擬機器上執行相同的實體電腦上的另一個伺服器基礎結構。 在此情況下網域、 安全性和網路的資料集中管理此伺服器基礎結構。 MultiPoint server 提供遠端桌面服務，並集中管理桌上型電腦。  
  
> [!NOTE]  
> 時的虛擬機器上執行 MultiPoint 服務，可支援 USB over 乙太網路，而 RDP 用戶端站台。 不支援直接的影片和 usb 極簡型用戶端連線的站台。  
  
如需有關 HYPER-V 角色的詳細資訊，請參閱 < [HYPER-V](../../virtualization/hyper-v/hyper-v-on-windows-server.md)。  
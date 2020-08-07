---
title: MultiPoint 服務虛擬化支援
description: 說明如何搭配使用 MultiPoint 服務與 Hyper-v
ms.date: 07/22/2016
ms.topic: article
ms.assetid: 3f0864b8-a087-4890-94ef-05efbd3c4241
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b2a07a71703887c9636cd02ca642be4cc4ddb47b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951803"
---
# <a name="multipoint-services-virtualization-support"></a>MultiPoint 服務虛擬化支援
MultiPoint 服務支援 Hyper-v 角色的方式有兩種：

-   MultiPoint 服務可以部署為執行 Hyper-v 之伺服器上的客體作業系統。

-   MultiPoint 服務可用來做為虛擬化伺服器。

在虛擬機器上執行 MultiPoint 服務，可讓您使用 Hyper-v 工具來管理作業系統。 這些工具組括檢查點和復原功能，並可讓您匯出和匯入虛擬機器。 對於較大的安裝，您可以在單一實體伺服器上執行多個 MultiPoint 服務虛擬電腦，以合併伺服器。 可能的案例包括：

-   單一教室或實驗室擁有20個以上的基座。 您可以在單一實體電腦上部署多個虛擬機器，而不是部署執行 MultiPoint 服務的多部實體電腦。

    > [!NOTE]
    > 您可以透過單一 MultiPoint 管理員主控台來管理多部 MultiPoint 伺服器，無論是實體或虛擬。

-   MultiPoint 伺服器正在虛擬機器上執行，並在同一部實體電腦上具有另一個伺服器基礎結構。 在此情況下，此伺服器基礎結構會將網路的網域、安全性和資料集中在一起。 MultiPoint 伺服器提供遠端桌面服務並集中化桌面。

> [!NOTE]
> 在虛擬機器上執行 MultiPoint 服務時，支援 USB over 乙太網路和 RDP 用戶端工作站。 不支援直接影片和 USB 零用戶端連線的工作站。

如需 Hyper-v 角色的詳細資訊，請參閱[hyper-v](../../virtualization/hyper-v/hyper-v-on-windows-server.md)。
---
title: 網路控制器可用性
description: 您可以使用本主題以深入了解 Network Controller 可用性的軟體定義網路 (SDN) 在 Windows Server 2016。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f260f3e4d8ca5fcd998824327478c2fbe3c81875
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller-high-availability"></a>網路控制器可用性

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要深入了解 Network Controller 可用性和軟體所定義網路 \(SDN\) 擴充性設定，您可以使用此主題。

當您在您的資料中心部署 SDN 時，您可以使用 Network Controller 集中部署、監視和管理許多網路項目，包括 RAS 閘道、軟體負載平衡器承租人通訊，Datacenter 防火牆原則，針對 SDN 原則、混合的網路原則，以及更多服務品質 \(QoS\) virtual 網路原則。

因為 Network Controller 的 SDN 管理基石，很重要的 Network Controller 部署提供可用性和的功能讓您輕鬆地縮放向上或向下 Network Controller 節點與您的資料中心需求。

雖然您可以部署 Network Controller 做為單一電腦叢集、可用性和容錯移轉您必須部署 Network Controller 在多部電腦叢集三電腦時最少提供。

>[!NOTE]
>您可以在其中一個伺服器電腦或虛擬的電腦是執行 Windows Server 2016 Datacenter edition \(VMs\) 部署 Network Controller。 如果您要部署網路上 Vm 的控制器，必須執行 Vm 的也執行 Datacenter edition HYPER-V 主機上。 Network Controller 並不適用於 Windows Server 2016 Standard edition。

## <a name="network-controller-as-a-service-fabric-application"></a>Network Controller 為服務 Fabric 應用程式

若要達到可用性和延展性，Network Controller 依賴 Fabric 服務。 服務 Fabric 提供建置延展性、可靠分散式的系統平台和輕鬆管理應用程式。

做為平台，服務 Fabric 會提供建置延展性分散式的系統所需的功能。 提供服務主機上多個作業系統執行個體，同步狀態之間執行個體，款領導者、偵測、負載平衡及更多的資訊。

>[!NOTE]
>在 Azure 服務 Fabric 有關，請查看[Azure 服務 Fabric 概觀](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview)。

在多部電腦上部署 Network Controller 時，Network Controller 執行單一服務 Fabric 應用程式與服務 Fabric 叢集上。 您可以藉由連接作業系統執行個體的一組構成服務 Fabric 叢集。

Network Controller 應用程式被組成狀態的多服務 Fabric 服務。 每個服務負責網路功能，例如管理實體網路，網路 virtual 管理、防火牆管理或閘道管理。 

每個服務 Fabric 服務有一個主要的複本，以及兩個次要複本。 主要服務複本處理要求，雖然的兩個次要服務複本提供可用性主要複本位置已停用或無法使用某些原因環境中。

下圖描述網路控制器服務 Fabric 叢集的五個的電腦。 四個服務視訊光碟五電腦上：防火牆服務、閘道服務、軟體負載平衡 \(SLB\) 服務，以及 virtual 網路 \(Vnet\) 服務。  每個四個服務包含一個主要服務複本和兩個次要服務複本。

![網路控制器服務 Fabric 叢集](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>使用服務 Fabric 優點

以下是使用網路控制器叢集服務 Fabric 主要優點。

### <a name="high-availability-and-scalability"></a>可用性和擴充性

Network Controller 的資料中心網路核心，因為它必須同時會失敗並調整達到 datacenter 網路中的快速地靈活變更允許時間。 下列功能提供下列功能： 

- **快速容錯移轉**。 服務 Fabric 提供出現極端快速容錯移轉。 多個熱次要服務複本都可供使用。 如果不硬體故障，您可以使用作業系統執行個體時，其中一個次要複本會立即升級主要複本。 
- **靈活度縮放比例的**。 您可以輕鬆快速地縮放操之在執行個體數千幾執行個體這些可靠服務，然後再回到幾案例中，根據您資源的需求。 

### <a name="persistent-storage"></a>永續性儲存空間

Network Controller 應用程式有大量儲存空間需求設定和狀態。 應用程式也必須使用上計劃與非預期關閉。 為這個項目的服務 Fabric 提供值金鑰存放區 \(KVS\) 的複製、交易與持續性的市集。

### <a name="modularity"></a>模組化

Network Controller 的設計與模組架構，與每個網路的服務，例如 virtual 網路服務防火牆服務，built\ 入為個人服務。 

此應用程式架構提供下列權益。

1. Network Controller 模組化做為可支援服務的每個獨立開發需要演進。 而不會影響的任何其他服務或 Network Controller 的正常運作，例如已更新的軟體負載平衡服務。
2. 網路控制器模組化允許的新的服務，在網路進化。 新的服務可以加入 Network Controller，而不影響現有的服務。

>[!NOTE]
>在 Windows Server 2016 不支援的第三方服務 Network Controller。

服務 Fabric 模組化以最大化開發、部署，及維護應用程式輕鬆使用服務模型結構描述。

## <a name="network-controller-deployment-options"></a>網路控制器部署選項

若要使用 System Center 一樣 Manager \(VMM\) 部署網路控制器，請查看[設定中 VMM fabric SDN 網路控制器](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller)。

若要部署 Network Controller 使用指令碼，查看[部署軟體定義網路基礎結構使用指令碼](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)。

若要部署使用 Windows PowerShell 網路控制器，請查看[使用 Windows PowerShell 部署 Network Controller](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)

如需 Network Controller 的詳細資訊，請查看[Network Controller](Network-Controller.md)。
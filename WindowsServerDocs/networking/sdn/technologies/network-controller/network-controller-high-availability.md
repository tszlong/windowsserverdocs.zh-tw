---
title: 網路控制卡高可用性
description: 若要了解網路控制站的高可用性的軟體定義網路 (SDN) Windows Server 2016 中，您可以使用本主題。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dbd3ae9f4c1f1fc3035fae9ace880046312df2f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813349"
---
# <a name="network-controller-high-availability"></a>網路控制卡高可用性

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來深入了解網路控制卡高可用性和延展性組態軟體定義網路\(SDN\)。

當您在資料中心部署 SDN 時，您可以使用網路控制卡集中部署、 監視和管理許多網路項目，包括 RAS 閘道，軟體負載平衡器、 虛擬網路的原則對於租用戶的通訊，資料中心防火牆原則的服務品質\(QoS\)的 SDN 原則、 混合式網路原則，以及更多。

由於網路控制站是 SDN 管理的基石，最重要的是網路控制卡部署，提供高可用性，以及讓您輕鬆地相應增加或減少網路控制卡節點具備您資料中心的需求。

雖然您可以部署為單一電腦叢集的網路控制站，高可用性和容錯移轉必須部署網路控制站在最少的三部機器的多個機器叢集。

>[!NOTE]
>您可以在任一伺服器電腦上或虛擬機器上部署網路控制卡\(Vm\)執行 Windows Server 2016 Datacenter edition。 如果您部署在 Vm 上的網路控制站時，Vm 必須也執行 Datacenter edition 的 HYPER-V 主機上執行。 無法在 Windows Server 2016 Standard edition 上使用網路控制站。

## <a name="network-controller-as-a-service-fabric-application"></a>網路控制站指定為 Service Fabric 應用程式

若要達到高可用性和延展性，網路控制站會依賴 Service Fabric。 Service Fabric 會提供一個分散式的系統平台建立可擴充、 可靠和易於管理的應用程式。

做為平台，Service Fabric 會提供所需建置可調整的分散式的系統功能。 它提供服務裝載於多個作業系統執行個體，同步處理之間執行個體中，選擇領導者，失敗偵測、 負載平衡，以及更多的狀態資訊。

>[!NOTE]
>在 Azure 中的 Service Fabric 的相關資訊，請參閱[Azure Service Fabric 概觀](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview)。

當您部署多部電腦上的網路控制站時，網路控制站將 Service Fabric 叢集上執行以單一的 Service Fabric 應用程式。 您可以藉由連接一組作業系統執行個體組成 Service Fabric 叢集。

網路控制站應用程式是由多個具狀態的 Service Fabric 服務所組成。 每個服務會負責網路功能，例如管理實體網路、 虛擬網路管理、 防火牆管理或閘道管理。 

每個 Service Fabric 服務有一個主要複本和兩個次要複本。 雖然兩個次要服務複本提供高可用性，在主要複本位置已停用或無法使用基於某些原因的情況下，主要服務複本會處理要求。

下圖說明使用五部機器的網路控制站的 Service Fabric 叢集。 四個服務會分散到五部的機器：防火牆服務，閘道服務，軟體負載平衡\(SLB\)服務和虛擬網路\(Vnet\)服務。  每四個服務包含一個主要服務複本和兩個次要服務複本。

![網路控制站的 Service Fabric 叢集](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>使用 Service Fabric 的優點

以下是針對網路控制卡的叢集使用 Service Fabric 的主要優點。

### <a name="high-availability-and-scalability"></a>高可用性和延展性

網路控制站是核心的資料中心網路，因為它必須同時能夠從失敗中恢復且可調整，以允許在資料中心網路中的敏捷式軟體開發的變更，經過一段時間。 下列功能提供這些功能： 

- **快速的容錯移轉**。 Service Fabric 提供極快速的容錯移轉。 多個熱門的次要服務複本一直都能。 如果作業系統執行個體因硬體失敗而無法使用，其中一個次要複本會立即升級至主要複本。 
- **標尺的靈活度**。 您可以輕鬆快速地調整這些可靠的服務，從高達數千個執行個體的少數執行個體，並接著向下到少數的執行個體，根據您的資源需求。 

### <a name="persistent-storage"></a>永續性儲存體

網路控制站應用程式具有其組態和狀態的大型儲存體需求。 應用程式也必須能夠使用在計劃性與非計劃性的中斷。 基於此目的，Service Fabric 提供索引鍵-值存放區\(KVS\) ，是複寫、 交易和持續性存放區。

### <a name="modularity"></a>模組化

網路控制站設計與模組化架構，與每個網路服務，例如虛擬網路服務與防火牆服務，建置\-中為個別的服務。 

此應用程式架構提供下列優點。

1. 模組化可獨立開發的每個支援的服務，讓做為網路控制站需求的發展。 比方說，可以更新的軟體負載平衡服務，而不影響任何其他服務或網路控制站正常運作。
2. 隨著網路的發展，網路控制器模組化可讓新增新的服務。 新的服務可以新增至網路控制站，而不會影響現有的服務。

>[!NOTE]
>在 Windows Server 2016 中，不支援新增至網路控制站的第三方服務。

Service Fabric 模組化會使用服務模型結構描述來最大化輕鬆開發、 部署和服務應用程式。

## <a name="network-controller-deployment-options"></a>網路控制站部署選項

若要使用 System Center Virtual Machine Manager 部署網路控制卡\(VMM\)，請參閱[設定 VMM 光纖中的 SDN 網路控制站](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller)。

若要部署網路控制站使用指令碼，請參閱[部署軟體定義網路基礎結構使用的指令碼](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)。

若要部署使用 Windows PowerShell 的網路控制站，請參閱[部署網路控制站使用 Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)

如需有關網路控制站的詳細資訊，請參閱[網路控制卡](Network-Controller.md)。
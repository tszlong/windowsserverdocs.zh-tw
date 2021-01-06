---
title: 網路控制卡高可用性
description: 您可以使用本主題來瞭解 Windows Server 2016 中軟體定義網路 (SDN) 的網路控制站高可用性。
manager: grcusanz
ms.topic: how-to
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: anpaul
author: AnirbanPaul
ms.date: 12/08/2020
ms.openlocfilehash: fe0967334e993c44d0edc38417998f51c8376262
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949104"
---
# <a name="network-controller-high-availability"></a>網路控制卡高可用性

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解軟體定義網路 SDN 的網路控制站高可用性和擴充性設定 \( \) 。

當您在資料中心部署 SDN 時，可以使用網路控制站來集中部署、監視及管理許多網路元素，包括 RAS 閘道、軟體負載平衡器、租使用者通訊的虛擬網路原則、資料中心防火牆原則、SDN 原則的服務品質 \( QoS \) 、混合式網路原則等等。

因為網路控制站是 SDN 管理的基石，所以網路控制站部署必須提供高可用性，而且您可以輕鬆地相應增加或相應減少網路控制站節點與您的資料中心需求。

雖然您可以將網路控制站部署為單一電腦叢集，但是針對高可用性和容錯移轉，您必須在多部電腦叢集中部署網路控制站，而且至少有三部電腦。

>[!NOTE]
>您可以在伺服器電腦或執行 \( \) Windows Server 2016 Datacenter edition 的虛擬機器 vm 上部署網路控制站。 如果您在 Vm 上部署網路控制站，Vm 必須在同時執行 Datacenter edition 的 Hyper-v 主機上執行。 Windows Server 2016 Standard edition 上無法使用網路控制卡。

## <a name="network-controller-as-a-service-fabric-application"></a>做為 Service Fabric 應用程式的網路控制站

為了達到高可用性和擴充性，網路控制站依賴 Service Fabric。 Service Fabric 提供分散式系統平臺來建立可擴充、可靠且易於管理的應用程式。

作為平臺，Service Fabric 提供建立可擴充的分散式系統所需的功能。 它提供裝載于多個作業系統實例上的服務、同步處理實例之間的狀態資訊、選取領導者、失敗偵測、負載平衡等等。

>[!NOTE]
>如需 Azure 中 Service Fabric 的詳細資訊，請參閱 [azure Service Fabric 的總覽](/azure/service-fabric/service-fabric-overview)。

當您在多部電腦上部署網路控制站時，網路控制站會在 Service Fabric 叢集上以單一 Service Fabric 應用程式的形式執行。 您可以藉由連接一組作業系統實例來形成 Service Fabric 叢集。

網路控制站應用程式是由多個具狀態的 Service Fabric 服務所組成。 每個服務都負責網路功能，例如實體網路管理、虛擬網路管理、防火牆管理或閘道管理。

每個 Service Fabric 服務都有一個主要複本和兩個次要複本。 主要服務複本會處理要求，而在主要複本基於某些原因而停用或無法使用的情況下，兩個次要服務複本會提供高可用性。

下圖說明具有五部機器 Service Fabric 叢集的網路控制卡。 四個服務分散在五部電腦上：防火牆服務、閘道服務、軟體負載平衡 \( SLB \) 服務和虛擬網路 \( Vnet \) 服務。  這四個服務中的每個都包含一個主要服務複本和兩個次要服務複本。

![網路控制卡 Service Fabric 叢集](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>使用 Service Fabric 的優點

以下是使用網路控制卡叢集 Service Fabric 的主要優點。

### <a name="high-availability-and-scalability"></a>高可用性和延展性

由於網路控制站是資料中心網路的核心，因此它必須能夠復原失敗，並可進行擴充，以允許一段時間內的資料中心網路進行 agile 變更。 下列功能提供下列功能：

- **快速容錯移轉**。 Service Fabric 可提供極快速的容錯移轉。 一律可使用多個經常性存取次要服務複本。 如果作業系統實例因為硬體失敗而變得無法使用，則其中一個次要複本會立即升級為主要複本。
- **調整規模的靈活性**。 您可以輕鬆快速地將這些可靠的服務從數個實例（最多數千個實例）調整為多個實例，然後根據您的資源需求向下調整到一些實例。

### <a name="persistent-storage"></a>永續性儲存體

網路控制站應用程式具有其設定和狀態的大型儲存體需求。 應用程式也必須可在計畫和未計畫的中斷之間使用。 基於這個目的，Service Fabric 提供了一種 Key-Value 儲存的 \( KVS \) ，也就是複寫的交易式和保存的存放區。

### <a name="modularity"></a>模組 化

網路控制站是以模組化架構來設計，每個網路服務，例如虛擬網路服務和防火牆服務， \- 內建為個別服務。

此應用程式架構提供下列優點。

1. 網路控制卡模組化可讓每個支援的服務獨立開發，隨需求演進。 例如，軟體負載平衡服務可以更新，而不會影響其他任何服務或網路控制站的正常操作。
2. 網路控制器模組化可讓您在網路演進時，新增新的服務。 新服務可以新增至網路控制卡，而不會影響現有的服務。

>[!NOTE]
>在 Windows Server 2016 中，不支援將協力廠商服務新增至網路控制站。

Service Fabric 模組化使用服務模型架構，可將應用程式的開發、部署和服務的便利性優化。

## <a name="network-controller-deployment-options"></a>網路控制站部署選項

若要使用 System Center Virtual Machine Manager VMM 來部署網路控制站 \( \) ，請參閱在 [vmm 網狀架構中設定 SDN 網路控制](/system-center/vmm/sdn-controller)站。

若要使用腳本部署網路控制站，請參閱 [使用腳本部署軟體定義的網路基礎結構](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)。

若要使用 Windows PowerShell 部署網路控制站，請參閱[使用 Windows PowerShell 部署網路控制](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)站

如需網路控制站的詳細資訊，請參閱 [網路控制](Network-Controller.md)站。

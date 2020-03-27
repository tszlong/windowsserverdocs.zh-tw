---
title: 網路控制卡高可用性
description: 您可以使用本主題來瞭解 Windows Server 2016 中軟體定義網路（SDN）的網路控制站高可用性。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ce3a0dd33ff105fa7cc36305048b8a311577aa21
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317060"
---
# <a name="network-controller-high-availability"></a>網路控制卡高可用性

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來瞭解網路控制卡的高可用性和擴充性設定，用於軟體定義的網路 \(SDN\)。

當您在資料中心部署 SDN 時，您可以使用網路控制站來集中部署、監視和管理許多網路元素，包括 RAS 閘道、軟體負載平衡器、租使用者通訊的虛擬網路原則、資料中心防火牆原則、SDN 原則的服務品質 \(QoS\)、混合式網路原則等等。

因為網路控制站是 SDN 管理的基石，所以網路控制站部署必須提供高可用性，而且能夠輕鬆地相應增加或相應減少網路控制站節點與您的資料中心需求，這是很重要的。

雖然您可以將網路控制站部署為單一機器叢集，但若要取得高可用性和容錯移轉，您必須在至少有三部電腦的多個電腦叢集中部署網路控制站。

>[!NOTE]
>您可以在伺服器電腦或虛擬機器上，部署執行 Windows Server 2016 Datacenter edition \(Vm\) 上的網路控制站。 如果您在 Vm 上部署網路控制站，Vm 必須在同時執行 Datacenter edition 的 Hyper-v 主機上執行。 網路控制卡無法在 Windows Server 2016 Standard edition 上使用。

## <a name="network-controller-as-a-service-fabric-application"></a>做為 Service Fabric 應用程式的網路控制站

為了達到高可用性和擴充性，網路控制站依賴 Service Fabric。 Service Fabric 提供分散式系統平臺，可建立可擴充、可靠且易於管理的應用程式。

做為平臺，Service Fabric 提供建立可擴充的分散式系統所需的功能。 它可在多個作業系統實例上提供服務裝載、在實例之間同步處理狀態資訊、選取領導者、失敗偵測、負載平衡等等。

>[!NOTE]
>如需有關 Azure 中 Service Fabric 的詳細資訊，請參閱[azure Service Fabric 的總覽](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview)。

當您在多部電腦上部署網路控制站時，網路控制卡會以單一 Service Fabric 應用程式的形式在 Service Fabric 叢集上執行。 您可以藉由連接一組作業系統實例來形成 Service Fabric 叢集。

網路控制卡應用程式是由多個具狀態 Service Fabric 服務所組成。 每項服務都負責網路功能，例如實體網路管理、虛擬網路管理、防火牆管理，或閘道管理。 

每個 Service Fabric 服務都有一個主要複本和兩個次要複本。 主要服務複本會處理要求，而兩個次要服務複本會在主要複本已停用或因為某些原因而無法使用的情況下，提供高可用性。

下圖說明網路控制卡 Service Fabric 具有五部機器的叢集。 四個服務會散佈在五部電腦上：防火牆服務、閘道服務、軟體負載平衡 \(SLB\) 服務，以及虛擬網路 \(Vnet\) 服務。  這四項服務中的每一個都包含一個主要服務複本和兩個次要服務複本。

![網路控制卡 Service Fabric 叢集](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>使用 Service Fabric 的優點

以下是使用網路控制卡叢集 Service Fabric 的主要優點。

### <a name="high-availability-and-scalability"></a>高可用性和擴充性

因為網路控制站是資料中心網路的核心，所以它必須能夠復原失敗，並可進行擴充，以允許一段時間內的資料中心網路進行 agile 變更。 下列功能可提供這些能力： 

- **快速容錯移轉**。 Service Fabric 提供極快速的容錯移轉。 多個經常性存取次要服務複本一律可供使用。 如果作業系統實例因為硬體故障而變得無法使用，則其中一個次要複本會立即升級為主要複本。 
- **規模的靈活性**。 根據您的資源需求，您可以輕鬆快速地將這些可靠的服務從幾個實例擴充到數千個實例，然後再移至幾個實例。 

### <a name="persistent-storage"></a>持續性儲存體

網路控制卡應用程式的設定和狀態有大型的儲存需求。 應用程式也必須可在已規劃和未計畫的中斷之間使用。 基於此目的，Service Fabric 會提供索引鍵/值存放區 \(KVS\)，也就是已複寫、交易式和保存的存放區。

### <a name="modularity"></a>模組

網路控制站的設計具有模組化架構，每個網路服務（例如虛擬網路服務和防火牆服務）都是以個別服務的形式在中建立\-。 

此應用程式架構提供下列優點。

1. 網路控制卡模組化可讓每個支援的服務獨立開發，因為需要進化。 例如，您可以更新軟體負載平衡服務，而不會影響網路控制站的任何其他服務或正常操作。
2. 網路控制站模組化允許新增服務，因為網路已發展。 新服務可以新增至網路控制站，而不會影響現有的服務。

>[!NOTE]
>在 Windows Server 2016 中，不支援將協力廠商服務新增至網路控制卡。

Service Fabric 模組化會使用服務模型架構，將應用程式的開發、部署和服務的便利性最大化。

## <a name="network-controller-deployment-options"></a>網路控制卡部署選項

若要使用 System Center Virtual Machine Manager \(VMM\)來部署網路控制站，請參閱在[vmm 網狀架構中設定 SDN 網路控制](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller)站。

若要使用腳本部署網路控制站，請參閱[使用腳本部署軟體定義網路基礎結構](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)。

若要使用 Windows PowerShell 部署網路控制站，請參閱[使用 Windows Powershell 部署網路控制](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)站

如需網路控制卡的詳細資訊，請參閱[網路控制](Network-Controller.md)卡。
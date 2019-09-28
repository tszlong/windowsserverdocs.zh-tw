---
title: 服務品質 (QoS) 原則
description: 本主題提供「服務品質」（QoS）原則的總覽，讓您可以使用群組原則來設定 Windows Server 2016 中特定應用程式和服務的網路流量頻寬優先順序。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 429c38d93c2c5c0053153d538304767c8261229c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395863"
---
# <a name="quality-of-service-qos-policy"></a>服務\(品質 QoS\)原則

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以透過建立 QoS 設定檔並使用群組原則發佈其設定，來使用 QoS 原則做為整體 Active Directory 基礎結構的網路頻寬管理中心點。

>[!NOTE]
>  除了本主題之外，還提供下列 QoS 原則檔。  
>   
>  - [使用 QoS 原則消費者入門](qos-policy-get-started.md)
>  - [管理 QoS 原則](qos-policy-manage.md)
>  - [QoS 原則的常見問題](qos-policy-faq.md)

QoS 原則會套用至使用者登入會話或電腦，做為您已連結至\(Active Directory\)容器（例如網域、網站或組織單位\(OU\)）的群組原則物件 GPO 的一部分。

QoS 流量管理會出現在應用層底下，這表示您現有的應用程式不需要修改，就能受益于 QoS 原則所提供的優點。

## <a name="operating-systems-that-support-qos-policy"></a>支援 QoS 原則的作業系統

您可以使用 QoS 原則來管理具有下列 Microsoft 作業系統之電腦或使用者的頻寬。

- Windows Server 2016
- Windows 10
- Windows Server 2012 R2
- Windows 8.1
- Windows Server 2012
- Windows 8
- Windows Server 2008 R2
- Windows 7
- Windows Server 2008
- Windows Vista

### <a name="location-of-qos-policy-in-group-policy"></a>QoS 原則在群組原則中的位置

在 Windows Server 2016 群組原則管理編輯器中，電腦設定的 QoS 原則路徑如下所示。

**預設網域原則 |電腦設定 |原則 |Windows 設定 |以\-原則為依據的 QoS**

下圖說明此路徑。

![QoS 原則在群組原則中的位置](../../media/QoS/QoS-Gp.jpg)

在 Windows Server 2016 群組原則管理編輯器中，[使用者設定] 的 QoS 原則路徑如下所示。

**預設網域原則 |使用者設定 |原則 |Windows 設定 |以\-原則為依據的 QoS**

預設不會設定任何 QoS 原則。

## <a name="why-use-qos-policy"></a>為何要使用 QoS 原則？
  
隨著網路的流量增加，您對網路效能與服務成本之間的平衡十分重要，但是網路流量通常不容易設定優先順序和管理。

在您的網路上\-，任務關鍵性\-和延遲敏感的應用程式必須針對較低優先順序的流量競爭網路頻寬。 同時，某些具有特定網路效能需求的使用者和電腦可能需要區分的服務等級。

提供符合成本效益且可預測的網路效能層級的挑戰通常會先出現在\(廣域網路\) WAN 連線或延遲敏感的應用程式（例如 voice over \(IP VoIP）上。\)和影片串流。 不過，提供可預測的網路服務層級的最終目標，適用于任何\(網路環境，例如，企業的區域網路\)，以及多個 VoIP 應用程式，例如您公司的自訂行商務\-應用程式。 \-
  
以原則為依據的 QoS 是一種網路頻寬管理工具，可讓您根據應用程式、使用者和電腦提供網路控制。 

當您使用 QoS 原則時，您的應用程式不需要針對特定應用程式開發介面\(api\)撰寫。 這讓您能夠將 QoS 與現有應用程式搭配使用。 此外，以原則為依據的 QoS 也會利用您現有的管理基礎結構，因為以原則為依據的 QoS 已內建在群組原則中。

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>透過差異服務的程式碼點\(DSCP 定義 QoS 優先順序\)
  
您可以使用指派給不同類型網路流量的差異服務碼點\(DSCP\)值，建立定義網路流量優先順序的 QoS 原則。 

DSCP 可讓您在 IPv4 封包\(標頭中\)的服務\(TOS\)欄位類型，以及 IPv6 的 [流量類別] 欄位內套用值0–63。 

DSCP 值會提供網際網路通訊協定\(IP\)層級的網路流量分類，路由器會使用此類別來決定流量佇列行為。 

例如，您可以設定路由器，將具有特定 DSCP 值的封包放入三個佇列的其中一個：高優先順序、最佳做法或低於最佳做法。 

高優先順序佇列中的任務關鍵性網路流量，具有其他流量的喜好設定。

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>限制每個應用程式的網路頻寬使用節流速率

您也可以在 QoS 原則中指定節流速率，以限制應用程式的輸出網路流量。

定義節流限制的 QoS 原則會決定輸出網路流量的速率。 例如，若要管理 WAN 成本，IT 部門可能會執行服務等級協定，指定檔案伺服器永遠不會提供超過特定速率的下載。  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>使用 QoS 原則來套用 DSCP 值和節流速率

您也可以使用 QoS 原則，將輸出網路流量的 DSCP 值和節流速率套用至下列各項：

- 正在傳送應用程式和目錄路徑

- 來源與目的地 IPv4 或 IPv6 位址或位址首碼

- 通訊協定-傳輸控制\(通訊\)協定 TCP 和使用者\(資料包通訊協定 UDP\)

- 來源和目的地埠，以及 TCP \(或 UDP 的埠範圍\)

- 在群組原則中透過部署的特定使用者或電腦群組

藉由使用這些控制項，您可以為 VoIP 應用程式指定具有 DSCP 值46的 QoS 原則，讓路由器能夠將 VoIP 封包放在低延遲的佇列中，或者您可以使用 QoS 原則來將一組伺服器的輸出流量節流到每秒 512 kb 從 TCP 通訊\)埠443傳送時，> KBps。

您也可以將 QoS 原則套用至具有特殊頻寬需求的特定應用程式。 如需詳細資訊，請參閱[QoS 原則案例](qos-policy-scenarios.md)。
  
## <a name="advantages-of-qos-policy"></a>QoS 原則的優點

透過 QoS 原則，您可以設定並強制執行無法在路由器和交換器上設定的 QoS 原則。 QoS 原則提供下列優點。
  
1. **詳細資料層級：** 很難以在路由器或交換器上建立使用者層級的 QoS 原則，特別是當使用者的電腦是使用動態 IP 位址指派來設定，或電腦未連線到固定交換器或路由器埠時，通常是使用可攜式電腦。 相較之下，QoS 原則可讓您更輕鬆地\-在網域控制站上設定使用者層級的 QoS 原則，並將原則傳播到使用者的電腦。
2. **彈性**。 無論電腦在何處或如何連線到網路，都會套用 QoS 原則-電腦可以從任何位置使用 WiFi 或 Ethernet 進行連線。 針對使用者\-層級的 QoS 原則，qos 原則會套用至使用者登入之任何位置的任何相容裝置上。
3. **安全級**如果您的 IT 部門使用網際網路通訊協定安全性\(IPsec\)從端對端加密使用者的流量，您就無法根據封包\(中 IP 層的任何資訊來分類路由器上的流量，例如TCP 通訊\)埠。 不過，藉由使用 QoS 原則，您可以將封包分類于終端裝置上，以在 IP 承載加密並傳送封包之前，指出 IP 標頭中的封包優先順序。
4. **性能**某些 QoS 函式（例如節流）較接近來源時，更能執行。 QoS 原則會將這類 QoS 函式移動到最接近來源的功能。
5. **功能**QoS 原則以兩種方式增強網路管理能力：

    **答**： 由於它是以群組原則為基礎，因此您可以使用 QoS 原則，視需要設定及管理一組使用者/電腦 QoS 原則，並在一部中央網域控制站電腦上。

    **b.** Qos 原則藉由提供一個機制來指定原則以統一資源定位器\(URL\) ，而不是根據 QoS 原則所需的每一部伺服器的 IP 位址指定原則，來協助使用者/電腦設定。要套用的。 例如，假設您的網路具有共用通用 URL 的伺服器叢集。 藉由使用 QoS 原則，您可以根據一般 URL 建立一個原則，而不是針對叢集中的每部伺服器建立一個原則，而是以每個伺服器的 IP 位址為基礎的每個原則。

如需本指南中的下一個主題，請參閱[使用 QoS 原則消費者入門](qos-policy-get-started.md)。


---
title: 服務品質 (QoS) 原則
description: 本主題提供服務品質 (QoS) 原則的總覽，可讓您使用群組原則來排定 Windows Server 2016 中特定應用程式和服務的網路流量頻寬。
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: dd43c38e44a02b3d50bff952b8a174b049c585d8
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946654"
---
# <a name="quality-of-service-qos-policy"></a>服務品質 \( QoS \) 原則

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以透過建立 QoS 設定檔並使用群組原則發佈其設定，來使用 QoS 原則做為整體 Active Directory 基礎結構的網路頻寬管理中心點。

>[!NOTE]
>  除了本主題之外，還提供下列 QoS 原則檔。
>
>  - [具有 QoS 原則的消費者入門](qos-policy-get-started.md)
>  - [管理 QoS 原則](qos-policy-manage.md)
>  - [QoS 原則的常見問題](qos-policy-faq.md)

QoS 原則會套用至使用者登入會話或電腦，做為 \( \) 您已連結至 Active Directory 容器（例如網域、網站或組織單位 OU）之群組原則物件 GPO 的一部分 \( \) 。

QoS 流量管理會發生在應用層下方，這表示您現有的應用程式不需要修改即可受益于 QoS 原則所提供的優點。

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

**預設網域原則 |電腦設定 |原則 |Windows 設定 |以原則為 \- 基礎的 QoS**

下圖說明此路徑。

![QoS 原則在群組原則中的位置](../../media/QoS/QoS-Gp.jpg)

在 Windows Server 2016 群組原則管理編輯器中，使用者設定的 QoS 原則路徑如下所示。

**預設網域原則 |使用者設定 |原則 |Windows 設定 |以原則為 \- 基礎的 QoS**

依預設，不會設定任何 QoS 原則。

## <a name="why-use-qos-policy"></a>為何要使用 QoS 原則？

當網路上的流量增加時，您必須在網路效能與服務成本之間取得平衡，但是網路流量通常不容易設定優先順序和管理。

在您的網路上，關鍵性 \- 和延遲 \- 敏感的應用程式必須與較低優先順序的流量競爭網路頻寬。 同時，某些具有特定網路效能需求的使用者和電腦可能需要差異化的服務層級。

提供符合成本效益且可預測的網路效能等級的挑戰通常會先出現在廣域網路 WAN 連線上， \( \) 或與延遲敏感的應用程式（例如透過 IP \( VoIP \) 和影片串流的語音）。 但是，提供可預測的網路服務等級的最終目標，會套用至任何網路環境，例如 \( 企業的區域網路 \) ，以及多個 VoIP 應用程式，例如您公司的自訂企業營運 \- \- 應用程式。

以原則為基礎的 QoS 是網路頻寬管理工具，可讓您根據應用程式、使用者和電腦提供網路控制。

當您使用 QoS 原則時，您的應用程式不需要針對特定的應用程式開發介面 \( api 撰寫 \) 。 這讓您能夠將 QoS 與現有的應用程式搭配使用。 此外，以原則為基礎的 QoS 會利用您現有的管理基礎結構，因為以原則為基礎的 QoS 內建在群組原則中。

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>透過差異服務程式碼點 DSCP 定義 QoS 優先順序 \(\)

您可以使用 \( \) 指派給不同類型網路流量的差異服務代碼點 DSCP 值，建立定義網路流量優先順序的 QoS 原則。

DSCP 可讓您在 \( \) IPv4 封包標頭中的 Service \( TOS 欄位類型內 \) ，以及 IPv6 的 [流量類別] 欄位中，套用值0到63。

DSCP 值提供網際網路通訊協定 IP 層級的網路流量分類 \( \) ，路由器會使用此層級來決定流量佇列行為。

例如，您可以設定路由器，將具有特定 DSCP 值的封包放入三個佇列中的其中一個：高優先順序、最大努力或低於最佳的工作。

高優先順序佇列中的要徑任務網路流量，其喜好設定高於其他流量。

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>使用節流速率限制每個應用程式的網路頻寬使用量

您也可以藉由指定 QoS 原則中的節流閥速率來限制應用程式的輸出網路流量。

定義節流限制的 QoS 原則會決定輸出網路流量的速率。 例如，若要管理 WAN 成本，IT 部門可能會執行服務等級協定，指定檔案伺服器絕對不能提供超過特定速率的下載。

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>使用 QoS 原則套用 DSCP 值和節流速率

您也可以使用 QoS 原則，將連出網路流量的 DSCP 值和節流速率套用至下列各項：

- 傳送應用程式和目錄路徑

- 來源和目的地 IPv4 或 IPv6 位址或位址首碼

- 通訊協定-傳輸控制通訊協定 \( TCP \) 和使用者資料包協定 \( UDP\)

- 來源和目的地埠和埠範圍（ \( TCP 或 UDP）\)

- 群組原則中部署的特定使用者或電腦群組

藉由使用這些控制項，您可以為 VoIP 應用程式指定一個 DSCP 值為46的 QoS 原則，讓路由器能夠將 VoIP 封包放在低延遲佇列中，或者您可以使用 QoS 原則， \( \) 在從 TCP 埠443傳送時，將一組伺服器的輸出流量節流至每秒 512 kb。

您也可以將 QoS 原則套用至具有特殊頻寬需求的特定應用程式。 如需詳細資訊，請參閱 [QoS 原則案例](qos-policy-scenarios.md)。

## <a name="advantages-of-qos-policy"></a>QoS 原則的優點

使用 QoS 原則，您可以設定和強制執行無法在路由器和交換器上設定的 QoS 原則。 QoS 原則提供下列優點。

1. **詳細資料層級：** 您很難在路由器或交換器上建立使用者層級的 QoS 原則，特別是當使用者的電腦是使用動態 IP 位址指派來設定的，或是電腦未連線至固定交換器或路由器埠（如同可攜式電腦經常發生的情況）。 相反地，QoS 原則可讓您更輕鬆地 \- 在網域控制站上設定使用者層級的 QoS 原則，並將原則傳播到使用者的電腦。
2. **彈性**。 無論電腦連線到網路的位置或方式為何，都會套用 QoS 原則，電腦可以從任何位置使用 WiFi 或乙太網路進行連接。 針對使用者 \- 層級的 qos 原則，qos 原則會套用到任何相容裝置的使用者登入位置。
3. **安全性：** 如果您的 IT 部門使用網際網路通訊協定安全性 IPsec，從端對端加密使用者的流量 \( \) ，您就無法根據封包中 IP 層上方的任何資訊 \( （例如，TCP 埠）來分類路由器上的流量 \) 。 不過，藉由使用 QoS 原則，您可以在終端裝置上分類封包，以在 IP 承載加密並傳送封包之前，指出 IP 標頭中的封包優先順序。
4. **效能：** 某些 QoS 功能（例如節流）較接近來源時更適合執行。 QoS 原則會移動最接近來源的 QoS 功能。
5. **管理能力：** QoS 原則以兩種方式增強網路管理能力：

    **答**： 由於它是以群組原則為基礎，因此您可以使用 QoS 原則來設定和管理一組使用者/電腦 QoS 原則，以及在一部中央網域控制站電腦上。

    **b.** QoS 原則可讓使用者/電腦設定藉由提供依統一資源定位器 URL 指定原則的機制， \( \) 而不是根據必須套用 QoS 原則的每一部伺服器的 IP 位址來指定原則。 例如，假設您的網路具有共用通用 URL 的伺服器叢集。 藉由使用 QoS 原則，您可以根據一般 URL 建立一項原則，而不是針對叢集中的每部伺服器建立一個原則，而是根據每部伺服器的 IP 位址來建立每個原則。

如需本指南的下一個主題，請參閱 [消費者入門與 QoS 原則](qos-policy-get-started.md)。


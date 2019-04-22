---
title: 服務品質 (QoS) 原則
description: 本主題概述的服務品質 (QoS) 原則，可讓您使用群組原則來設定特定的應用程式和服務 Windows Server 2016 中的網路流量頻寬優先順序。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a3ef81825ef6544bc96506e37bc027e0ac2a78db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820279"
---
# <a name="quality-of-service-qos-policy"></a>服務品質\(QoS\)原則

>適用於：Windows Server （半年通道），Windows Server 2016

您可以透過建立 QoS 設定檔並使用群組原則發佈其設定，來使用 QoS 原則做為整體 Active Directory 基礎結構的網路頻寬管理中心點。

>[!NOTE]
>  本主題中，除了下列的 QoS 原則文件使用。  
>   
>  - [開始使用 QoS 原則](qos-policy-get-started.md)
>  - [管理 QoS 原則](qos-policy-manage.md)
>  - [QoS 原則常見問題集](qos-policy-faq.md)

QoS 原則會套用到使用者登入工作階段或電腦群組原則物件的一部分\(GPO\)已連結到 Active Directory 容器，例如網域、 網站或組織單位\(OU\)。

下列應用程式層，這表示，您現有的應用程式不需要修改，才能受益於 QoS 原則所提供的優點，就會發生 QoS 流量管理。

## <a name="operating-systems-that-support-qos-policy"></a>支援 QoS 原則的作業系統

若要管理的電腦或使用下列 Microsoft 作業系統的使用者頻寬，您可以使用 QoS 原則。

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

### <a name="location-of-qos-policy-in-group-policy"></a>群組原則中的 QoS 原則的位置

在 Windows Server 2016 群組原則管理編輯器] 中的 [電腦設定 QoS 原則的路徑如下。

**預設網域原則 |電腦設定 |原則 |Windows 設定 |原則\-基礎 QoS**

下圖說明此路徑。

![群組原則中的 QoS 原則的位置](../../media/QoS/QoS-Gp.jpg)

在 Windows Server 2016 群組原則管理編輯器中，使用者設定 QoS 原則的路徑如下。

**預設網域原則 |使用者設定 |原則 |Windows 設定 |原則\-基礎 QoS**

預設會不設定任何 QoS 原則。

## <a name="why-use-qos-policy"></a>為何要使用 QoS 原則？
  
隨著流量增加您的網路上，為您的服務層的網路效能之間取得平衡日趨重要，但網路流量不正常容易排定優先順序以及管理。

在您的網路上的任務\-關鍵和延遲\-機密應用程式必須爭取對較低優先順序流量的網路頻寬。 在此同時，某些使用者和電腦特定的網路效能需求可能需要將不同服務層級。

提供符合成本效益且可預測的網路效能層級的挑戰通常第一次出現廣域網路\(WAN\)連線或延遲的應用程式，例如 voip \(VoIP\)和視訊串流。 不過，提供可預測的網路服務層級的最終目標套用至任何網路環境\(例如，企業的區域網路\)，以及多個 VoIP 的應用程式，例如您公司的自訂企業\-的\-商務應用程式。
  
原則為依據的 QoS 會為您提供網路控制-根據應用程式、 使用者和電腦的網路頻寬管理工具。 

當您使用 QoS 原則時，您的應用程式不需要針對特定的應用程式開發介面撰寫\(Api\)。 這會讓您能夠使用現有的應用程式中的 QoS。 此外，原則為依據的 QoS 會利用現有的管理基礎結構，因為以原則為基礎的 QoS 會內建在群組原則。

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>定義透過差異化的服務字碼指標的 QoS 優先順序\(DSCP\)
  
您可以建立 QoS 原則，定義網路流量優先順序與 Differentiated Services 字碼指標\(DSCP\)您指派給不同類型的網路流量的值。 

DSCP 可讓您將值套用\(0 – 63\)類型的服務內\(TOS\) IPv4 封包的標頭，並在 IPv6 Traffic Class 欄位內的欄位。 

DSCP 值提供在網際網路通訊協定的網路流量分類\(IP\)層級，決定流量的佇列行為中使用的路由器。 

例如，您可以設定路由器，以將具有特定 DSCP 值的封包放入三個佇列的其中一個： 高優先順序，盡力應用程式，或低於最大努力。 

任務關鍵性的網路流量，也就是高優先順序佇列中，具有比其他流量的喜好設定。

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>每個應用程式與節流閥速率限制網路頻寬用量

您也可以在 QoS 原則中指定的節流閥速率，以限制應用程式的輸出網路流量。

QoS 原則，以定義節流限制會決定輸出網路流量的速率。 比方說，為了管理廣域網路的成本，IT 部門可能會實作指定的檔案伺服器可以永遠不會提供特定的速率超過下載的服務等級協定。  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>使用 QoS 原則，來套用 DSCP 值和節流閥速率

您也可以使用 QoS 原則，以套用 DSCP 值和節流，費率如下的輸出網路流量：

- 傳送應用程式和目錄路徑

- 來源和目的地 IPv4 或 IPv6 位址或位址首碼

- 通訊協定-傳輸控制通訊協定\(TCP\)和使用者資料包通訊協定\(UDP\)

- 來源和目的地連接埠和連接埠範圍\(TCP 或 UDP\)

- 特定的使用者或電腦透過群組原則中的部署群組

藉由使用這些控制項，您可以指定 QoS 原則，DSCP 值是 46 VoIP 應用程式，讓路由器能夠將 VoIP 封包放在低延遲佇列中，或您可以使用 QoS 原則，以啟用節流設定一組伺服器的輸出流量至 512 kb / 秒<c1/>KBps\)時從 TCP 連接埠 443 傳送。

您也可以套用 QoS 原則有特殊的頻寬需求的特定應用程式。 如需詳細資訊，請參閱 < [QoS 原則案例](qos-policy-scenarios.md)。
  
## <a name="advantages-of-qos-policy"></a>QoS 原則的優點

QoS 原則，您可以設定和強制執行 QoS 原則，無法在路由器和交換器上設定。 QoS 原則可提供下列優點。
  
1. **層級的詳細資料：** 很難建立路由器或交換器上的使用者層級 QoS 原則，特別是如果使用者的電腦是使用動態 IP 位址指派設定，或的電腦未連線至固定的交換器或路由器連接埠，因為是經常使用的情況可攜式電腦。 相反地，QoS 原則可讓您更輕鬆地設定使用者\-層級的網域控制站上的 QoS 原則，並傳播至使用者的電腦原則。
2. **彈性**。 無論何處或如何將電腦連線到網路，請套用 QoS 原則-電腦可以從任何位置使用 WiFi 或乙太網路連線。 使用者\-層級的 QoS 原則，QoS 原則會套用至任何相容的裝置，在使用者登入的任何位置。
3. **安全性：** 如果您的 IT 部門會將使用者流量從端對端加密使用網際網路通訊協定安全性\(IPsec\)，您無法將分類的封包中的 IP 層上的任何資訊為基礎的路由器上的流量\(比方說，TCP 連接埠\)。 不過，藉由使用 QoS 原則，您可以將結束的裝置，以指出 IP 標頭中的封包的優先順序之前已經加密的 IP 裝載，並傳送封包, 的封包。
4. **效能：** QoS 函式，例如節流、 更接近來源時執行。 QoS 原則會將這類來源最接近的 QoS 函式。
5. **管理能力：** QoS 原則會增強網路管理功能，有兩種：

    。 因為它以群組原則為基礎，您可以使用 QoS 原則，若要設定及管理一組使用者/電腦 QoS 原則，每當有需要，和一個中央的網域控制站電腦上。

    **b**。 QoS 原則可協助使用者/電腦設定，藉由提供一個機制來指定原則的統一資源定位器\(URL\)而不是指定每個 QoS 原則，需要的伺服器的 IP 位址為基礎的原則若要套用。 例如，假設您的網路共用通用的 URL 的伺服器叢集。 藉由使用 QoS 原則，您可以建立一個常見的 URL，而不是建立在叢集中，每個伺服器的一項原則，根據每一部伺服器的 IP 位址的每個原則為基礎的原則。

如本指南中的下一個主題，請參閱 < [Getting Started with QoS 原則](qos-policy-get-started.md)。


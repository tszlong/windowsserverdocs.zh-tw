---
title: 品質的服務 (QoS) 原則
description: 本主題提供概觀品質服務 (QoS) 原則，讓您可以使用群組原則優先順序網路流量頻寬特定應用程式和 Windows Server 2016 中的服務。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 76405c677fe7eb4d51f9c190499b909ac2fe4a0c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="quality-of-service-qos-policy"></a>品質的服務 \(QoS\) 原則

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用 QoS 原則的網路頻寬管理的中心點整體 Active Directory 基礎結構建立 QoS 設定檔，視訊光碟使用群組原則設定。

>[!NOTE]
>  本主題中，除了下列 QoS 原則文件會提供。  
>   
>  - [開始使用 QoS 原則](qos-policy-get-started.md)
>  - [管理 QoS 原則](qos-policy-manage.md)
>  - [QoS 原則常見問題集](qos-policy-faq.md)

QoS 原則套用到工作階段使用者登入或電腦的群組原則物件的一部分有與您連結到網域、網站或組織單位 Active Directory 容器的 \(GPO\) \(OU\)。

下列應用程式層，這表示您現有的應用程式，不需要修改受益所提供的 QoS 原則優點，就會發生 QoS 流量管理。

## <a name="operating-systems-that-support-qos-policy"></a>支援 QoS 原則作業系統

管理適用於 Microsoft 下列作業系統的使用者或電腦的頻寬，您可以使用 QoS 原則。

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

### <a name="location-of-qos-policy-in-group-policy"></a>群組原則中 QoS 原則的位置

Windows Server 2016 群組原則管理編輯器] 中的 [電腦設定 QoS 原則的路徑如下。

**預設網域原則 |電腦設定 |原則 |Windows 設定 |Policy\ 式 QoS**

這個路徑是在下列影像中所示。

![群組原則中 QoS 原則的位置](../../media/QoS/QoS-Gp.jpg)

Windows Server 2016 群組原則管理編輯器] 中的使用者設定原則 QoS 路徑如下。

**預設網域原則 |使用者設定 |原則 |Windows 設定 |Policy\ 式 QoS**

預設不 QoS 原則設定。

## <a name="why-use-qos-policy"></a>為何使用 QoS 原則嗎？
  
為在您的網路流量，越來越重要，以平衡服務-的費用與網路效能，但網路流量不正常容易優先順序和管理。

在您的網路，mission\ 重大和 latency\ 顧慮應用程式必須的頻寬對較低優先順序流量進行。 在此同時，有些使用者及特定的網路可能需要需求的效能與電腦區分服務層級。

提供具成本效益、預測的網路效能層級的挑戰通常第一次顯示透過寬區域網路 \(WAN\) 連接或延遲敏感的應用程式，例如 IP \(VoIP\) 和影片的串流語音。 不過，提供可預測網路服務層級結束目標適用於任何網路環境 \ (例如企業的區域 network\)，以及一個以上的 VoIP 應用程式，例如您的公司自訂 line\ of\ 商務應用程式。
  
原則為主 QoS 是網路的控制項-型應用程式、使用者和電腦上為您提供的網路頻寬管理工具。 

當您使用 QoS 原則時，不需要針對特定應用程式的程式設計介面 \(APIs\) 撰寫您的應用程式。 這可讓您以 QoS 使用現有的應用程式。 此外，原則為主 QoS 利用您現有的管理基礎結構，所以原則為主 QoS 至群組原則。

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>定義透過已區分服務的程式碼點 QoS 優先順序 \(DSCP\)
  
您可以建立定義 Differentiated 服務的程式碼的端點 \(DSCP\) 值，您將指派到不同的網路流量的網路流量優先順序 QoS 原則。 

DSCP 可讓您套用值 \(0–63\) 流量課程中的欄位 IPv6 和中輸入的服務 \(TOS\) 中的欄位標頭 IPv4 封包。 

DSCP 值提供網際網路通訊協定 \(IP\) 級路由器，進而佇列行為流量是使用網路流量分類。 

例如，您可以設定要使用特定 DSCP 值封包放其中一個三個佇列路由器：為高優先順序最佳努力，或低於最佳效果。 

重要的網路流量是設定為高優先順序佇列中，會有其他資料傳輸到喜好設定。

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>限制網路頻寬使用每個節流閥速率的應用程式

您也可以透過節流閥速率指定 QoS 原則中限制輸出網路流量的應用程式。

定義節流限制 QoS 原則判斷輸出網路流量的頻率。 例如，管理 WAN 費用，IT 部門，取得可能實作服務的層級合約指定的檔案伺服器可以永遠不會提供下載特定的速率以外的。  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>使用適用於 DSCP QoS 原則值，並調節率

您也可以使用原則 QoS 套用 DSCP 值和調節下列輸出網路流量的速率：

- 應用程式傳送和路徑

- 來源和目的地 IPv4 或 IPv6 位址或地址前置詞

- 通訊協定-傳輸控制項 \(TCP\) 及使用者資料流通訊協定 \(UDP\) 通訊協定

- 來源和目的地連接埠和連接埠範圍 \(TCP or UDP\)

- 特定的使用者或群組原則中部署到電腦群組

使用這些控制項，您可以指定 QoS 原則 DSCP 值為 46 VoIP 應用程式，讓在低延遲佇列中，將 VoIP 封包路由器或您可以壓制設定伺服器的第二個 \(KBps\) 每 512 kb 輸出流量的 TCP 連接埠 443 傳送時使用 QoS 原則。

您也可以套用原則 QoS 特定應用程式的特殊頻寬需求。 如需詳細資訊，請查看[QoS 原則案例](qos-policy-scenarios.md)。
  
## <a name="advantages-of-qos-policy"></a>QoS 原則的優點

使用 QoS 原則，您可以設定，並執行 QoS 原則，無法在路由器，並切換設定。 QoS 原則提供下列優點。
  
1. **詳細資訊層級︰**很難建立路由器或切換使用者層級 QoS 原則，尤其是使用者的電腦是可能可以使用動態 IP 設定位址指派或電腦無法連接到修正的切換或路由器連接埠，如果是經常使用筆記型電腦案例。 相較之下，QoS 原則可讓您更輕鬆地使用者 \ 層級 QoS 原則設定的網域控制站在並將傳送至電腦的使用者的原則。
2. **彈性**。 何處，或如何電腦連接到網路，無論 QoS 原則-電腦可以透過 wifi 下載或乙太網路從任何位置。 使用者 \ 層級 QoS 原則，QoS 原則會套用至任何相容的裝置在使用者登入的任何位置。
3. **安全性：** IT 部門加密使用者流量的端點使用網際網路通訊協定的安全性 \(IPsec\)，如果您無法將分類根據上述 IP 層級封包中的任何資訊路由器上的流量 \ (例如，TCP port\)。 不過，您可以使用 QoS 原則，可結束裝置之前的 IP 裝載加密，並傳送封包指出中 IP 標頭的封包優先順序封包。
4. **效能：**某些 QoS 功能，例如節流、變得更好靠近來源時執行。 QoS 原則移動靠近來源這類 QoS 函式。
5. **管理性：** QoS 原則美化網路管理兩種方式：

    **a**. 因為它根據群組原則，您可以使用 QoS 原則設定及管理使用者日電腦 QoS 原則一組時所需，以及一個中央網域控制站電腦上。

    **b**. 藉由提供機制，指定原則，而不會指定每個 QoS 原則要套用的伺服器的 IP 位址所根據原則統一資源定位器 \(URL\) QoS 原則幫助您日電腦的使用者設定。 例如，假設您的網路分享常見的 URL 伺服器叢集。 使用 QoS 原則，您可以建立一個原則根據常見的 URL，而非建立一個原則每個伺服器叢集，在與每個原則根據每個伺服器的 IP 位址。

本指南下一步主題，請查看[開始 QoS 原則的](qos-policy-get-started.md)。


---
title: QoS 原則案例
description: 本主題提供品質服務 (QoS) 原則案例，示範如何使用群組原則的特定應用程式和 Windows Server 2016 的服務的網路流量的優先順序。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b635f73cc25552f4c1a05f8db6303f636eda3c54
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-scenarios"></a>QoS 原則案例

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以檢視假設案例示範如何的時機，以及如何使用 QoS 原則。

此主題中的兩個案例︰

1. 優先順序網路流量的業務應用程式
2. 伺服器 HTTP 應用程式的優先順序網路流量

>[!NOTE]
>本主題的一些一節包含一般的步驟可讓您執行上述的動作。 如需詳細管理 QoS 原則的指示，請查看[管理 QoS 原則](qos-policy-manage.md)。

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>案例 1：優先順序網路流量的業務應用程式

在本案例中，IT 部門，取得會有數種目標，他們可以使用原則 QoS 完成：

- 提供更好的網路效能的 mission\ 重要的應用程式。
- 使用特定應用程式時，提供更佳的一組金鑰使用者的網路效能。
- 請確定 company\ 全資料備份應用程式不會網路效能阻礙一次使用太多頻寬。

設定優先順序特定應用程式使用區分服務的程式碼點 \(DSCP\) 值分類網路流量，並設定為高優先順序流量的優先處理其路由器 QoS 原則決定 IT 部門。 

>[!NOTE]
>DSCP 的詳細資訊，會看到一節**定義 QoS 優先順序透過區分服務的程式碼點**主題中的[原則品質服務 (QoS)](qos-policy-top.md)。

除了 DSCP 值，QoS 原則可以指定節流閥速率。 節流有限制相符項目 QoS 原則的特定傳送速率所有輸出流量的效果。

### <a name="qos-policy-configuration"></a>QoS 原則設定

完成三個不同的目標，以建立三個不同的 QoS 原則決定 IT 系統管理員。

#### <a name="qos-policy-for-lob-app-servers"></a>LOB 應用程式伺服器 QoS 原則

第一次 mission\ 重大應用程式的 IT 部門建立 QoS 原則是 company\ 全企業資源規劃 \(ERP\) 應用程式。 幾個所有執行 Windows Server 2016 的電腦上裝載 ERP 應用程式。 Active Directory Domain Services，這些電腦將組織單位成員所建立的業務 \(LOB\) 應用程式伺服器 \(OU\)。 Client\ 端元件 ERP 應用程式正在執行 Windows 10 和「Windows 8.1 的電腦上安裝。

在群組原則中，IT 系統管理員，請選取群組原則物件 \(GPO\) 時，將會套用 QoS 原則。 利用 QoS 原則精靈，IT 系統管理員會建立 QoS 原則稱為「伺服器 LOB 原則」，指定 high\ 優先順序 DSCP 值 44 的所有應用程式，任何 IP 位址、TCP 與 UDP，以及連接埠號碼。

QoS 原則只會套用到 LOB 伺服器將 GPO 連結到組織單位，其中包含只這些伺服器，透過群組原則管理主控台 \(GPMC\) 工具。 電腦傳送網路流量時，此初始伺服器 LOB 原則適用於 high\ 優先順序 DSCP 值。 您可以稍後編輯 QoS 這項原則 \（在 [群組原則物件編輯器 tool\) 包含 ERP 應用程式的連接埠號碼限制套用只有在使用指定連接埠號碼時才原則。

#### <a name="qos-policy-for-the-finance-group"></a>QoS 財經群組原則

多個群組中公司存取 ERP 應用程式，而財經群組此應用程式而定處理針對時, 和群組需要一直都很高的應用程式的效能。

若要確保財經群組，可支援他們針對，QoS 原則必須為高優先順序分類這些使用者資料傳輸。 不過，財經群組成員使用以外 ERP 應用程式的應用程式時，應該不適用於原則。 

因為，IT 部門定義第二個 QoS 原則稱為「Client LOB 原則」在群組原則物件編輯器工具時，套用 60 DSCP 值財經使用者群組執行 ERP 應用程式。

#### <a name="qos-policy-for-a-backup-app"></a>適用於備份的 App QoS 原則

另一個備份應用程式正在執行的所有電腦上。 若要確保流量備份應用程式不會使用的所有可用的網路資源，IT 部門建立備份資料原則。 這項原則備份指定 DSCP 1 備份的 app，也就是可執行檔的名稱為基礎的值**backup.exe**。 

第三方 GPO 建立並針對網域中的所有 client 電腦部署。 每當您備份的應用程式傳送的資料，會套用低優先順序 DSCP 值，即使來源的財經部的電腦。
  
>[!NOTE]
>網路流量 QoS 原則不會傳送與 DSCP 設定為 0。

### <a name="scenario-policies"></a>案例原則

下表摘要 QoS 原則本案例。
  
|原則的名稱|DSCP 值。|調節率|套用到組織單位|描述|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[無 policy]|0|無|[無的部署]|適用於未分類流量最佳努力（預設值）處理。|  
|備份資料|1|無|所有戶端|適用於此大量資料低優先順序 DSCP 值。|  
|伺服器 LOB|44|無|電腦組織單位 ERP 伺服器|適用於 ERP 伺服器流量高優先順序 DSCP|  
|Client LOB|60|無|財經使用者群組|適用於 ERP client 流量高優先順序 DSCP|  

>[!NOTE]
>在小數點表單表示 DSCP 值。

有了 QoS 原則定義，並使用群組原則來套用，輸出網路流量接收原則指定 DSCP 值。 路由器會再提供差異處理使用佇列，根據這些 DSCP 值。 使用四個佇列此 IT 部門，設定路由器：高優先順序、中央優先順序、最佳效能，並低優先順序。

交通到達 DSCP 值路由器」伺服器 LOB 原則」和「Client LOB 原則，」資料放入高優先順序佇列。 使用 DSCP 設定為 0 流量接收服務最佳層級。 使用 DSCP 值（從備份應用程式）1 封包收到低優先順序處理。  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>必要條件排列業務的應用程式

若要完成這項工作，請確定您符合下列需求：

- 執行相容 QoS\ 作業系統涉及的電腦。

- 涉及的電腦是 Active Directory Domain Services \(AD DS\) 網域的成員，他們可以使用群組原則設定。

- TCP/IP 網路是設定為 DSCP \(RFC 2474\) 路由器設定。 如需詳細資訊，請查看[RFC 2474](http://www.ietf.org/rfc/rfc2474.txt)。

- 符合系統管理員認證需求。

#### <a name="administrative-credentials"></a>管理認證

若要完成這項工作時，您必須建立及部署群組原則物件。
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>設定設定優先順序業務的應用程式的測試環境

若要設定測試環境，請完成下列工作。

- 建立 AD DS 網域戶端與分為組織單位使用者。 如需部署 AD DS 指示，請查看[核心網路指南](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide)。

- 設定路由器，以 differentially 佇列根據 DSCP 值。 例如，DSCP 值 44 進入「白金」佇列和其他所有人都偏-公平佇列。

>[!NOTE]
> 您可以使用工具，例如網路監視器網路擷取檢視 DSCP 值。 執行網路擷取之後，您可以看到 TO 中的欄位擷取的資料。

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>步驟排列業務的應用程式

若要優先順序業務的應用程式，請完成下列工作：

1. 建立並連結 QoS 原則的群組原則物件 \(GPO\)。

2. 設定 differentially 將-業務路由器選取 DSCP 值為基礎的應用程式（由使用佇列）。 這項工作的程序會根據您的路由器類型而有所不同。

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>案例 2: HTTP 伺服器應用程式的優先順序網路流量

在 Windows Server 2016 原則為主 QoS 包括 URL 型原則的功能。 URL 原則可讓您管理頻寬 HTTP 伺服器。

許多企業應用程式的開發裝載網際網路資訊服務 \(IIS\) web 伺服器，並 Web 應用程式存取的瀏覽器 client 電腦上。

在本案例中，假設您管理一組 IIS 伺服器該主機訓練影片適用於您組織的所有員工。 您的目標是確認這些視訊伺服器的流量將不會使不勝負荷您的網路，並確認的視訊流量從在網路上的語音和資料傳輸區分。 

任務是類似案例 1 中的工作。 您將會設計和設定流量管理設定，例如 DSCP 值流量影片，並節流評分相同像您的業務應用程式。 但指定傳輸，而不提供的應用程式名稱、時僅輸入的 URL 的回應 HTTP 伺服器應用程式將會：例如，https://hrweb/training。
  
> [!NOTE]
>您無法使用 URL 型 QoS 原則網路流量的電腦執行的 Windows 作業系統的發行前 Windows 7 和 Windows Server 2008 R2 的優先順序。

### <a name="precedence-rules-for-url-based-policies"></a>優先順序規則 URL 型原則

所有下列 Url 有效，並且可以控制台中 QoS 原則和同時套用到電腦或使用者：

- http://video

- https://training.hr.mycompany.com

- http://10.1.10.249:8080/tech  

- https://*/ebooks

但是，是哪一種將會收到優先順序嗎？ 很簡單的規則。 向右朗讀訂單中的 URL 型原則的優先順序。 因此，從優先順序最低到最高優先順序，URL 欄位︰
  
[1.URL 配置](#bkmk_QoS_UrlScheme)

[2.URL 主機](#bkmk_QoS_UrlHost)

[3.URL 連接埠](#bkmk_QoS_UrlPort)

[4.URL 路徑](#bkmk_QoS_UrlPath)

以下是詳細資料：

####  <a name="bkmk_QoS_UrlScheme"></a>1.URL 配置

 `https://` 較高優先順序比`http://`。

####  <a name="bkmk_QoS_UrlHost"></a>2.URL 主機

 從最低到最高優先順序，它們會：

1. 主機

2. IPv6 位址

3. IPv4 位址

4. 萬用字元

在主機，點更多項目（深入）主機名稱已高於主機使用較少點的項目名稱的優先順序。 例如，在下列主機：

- video.internal.training.hr.mycompany.com (深度 = 6)
  
- selfguide.training.mycompany.com (深度 = 4)
  
- 訓練 (深度 = 1 台)
  
- 媒體櫃 (深度 = 1 台)
  
 **video.internal.training.hr.mycompany.com**最高優先順序，並**selfguide.training.mycompany.com**下一步的最高優先順序。 **訓練**與**媒體櫃**共用相同最低的優先順序。  
  
####  <a name="bkmk_QoS_UrlPort"></a>3.URL 連接埠

特定或隱含的連接埠號碼已高於萬用字元連接埠的優先順序。

####  <a name="bkmk_QoS_UrlPath"></a>4.URL 路徑

主機名稱，例如多個項目可能會包含 URL 路徑。 有多個項目都高於較少的優先順序。 例如，下列路徑列出的優先順序：  

1.  /ebooks/tech/windows/networking/qos

2.  日有聲月 tech 日 windows 日

3.  /ebooks

4.  /

如果使用者選擇以包含所有子目錄和下列 URL 路徑檔案，這個 URL 路徑必須較低優先順序比它不做選擇如果。

也可以選擇使用者 URL 原則中指定的目的地 IP 位址。 目的地 IP 位址比任何之前所述的四個 URL 欄位較低優先順序。
  
### <a name="quintuple-policy"></a>5 倍原則

通訊協定 ID、來源 IP 位址、來源連接埠，目的地 IP 位址，以及目的地連接埠，指定 5 倍原則。 5 倍原則一定有優先順序高於任何 URL 為基礎的原則。 

如果 5 倍原則已經套用的使用者，新的 URL 型原則不會導致衝突使用者的 client 的任何上的電腦。

本指南下一步主題，請查看[管理 QoS 原則](qos-policy-manage.md)。

本指南中第一次主題，請查看[品質服務 (QoS) 原則](qos-policy-top.md)。

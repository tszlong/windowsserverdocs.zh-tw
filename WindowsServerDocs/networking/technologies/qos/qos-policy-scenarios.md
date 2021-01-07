---
title: QoS 原則案例
description: 本主題提供服務品質 (QoS) 原則案例，示範如何使用群組原則來優先處理 Windows Server 2016 中特定應用程式和服務的網路流量。
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: f1a3b76c0939e0b6dbe445799bdc9c96c4f16f1f
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950384"
---
# <a name="qos-policy-scenarios"></a>QoS 原則案例

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來查看示範如何、何時以及為何使用 QoS 原則的假設案例。

本主題的兩個案例如下：

1. 排定企業營運應用程式的網路流量優先順序
2. 設定 HTTP 伺服器應用程式網路流量的優先順序

>[!NOTE]
>本主題的某些章節包含執行所述動作所能採取的一般步驟。 如需管理 QoS 原則的詳細指示，請參閱 [管理 Qos 原則](qos-policy-manage.md)。

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>案例1：排列企業營運應用程式的網路流量優先順序

在此案例中，IT 部門有幾個可使用 QoS 原則完成的目標：

- 為任務關鍵性應用程式提供更佳的網路效能 \- 。
- 當使用者使用特定應用程式時，為一組重要的使用者提供更好的網路效能。
- 確定全公司的 \- 資料備份應用程式不會因為一次使用太多頻寬而妨礙網路效能。

IT 部門決定將 QoS 原則設定為使用差異服務代碼點 \( DSCP \) 值來分類網路流量，並設定其路由器為較高優先順序的流量提供優先處理，以設定特定應用程式的優先順序。

>[!NOTE]
>如需 DSCP 的詳細資訊，請參閱「[服務品質 (qos) 原則](qos-policy-top.md)」主題中的「**透過差異服務程式碼點定義 QoS 優先順序**」一節。

除了 DSCP 值之外，QoS 原則還可以指定節流閥速率。 節流具有將符合 QoS 原則的所有輸出流量限制為特定傳送速率的影響。

### <a name="qos-policy-configuration"></a>QoS 原則設定

IT 系統管理員決定建立三個不同的 QoS 原則，以達成三個不同的目標。

#### <a name="qos-policy-for-lob-app-servers"></a>LOB 應用程式伺服器的 QoS 原則

\-IT 部門建立 QoS 原則的第一個任務關鍵性應用程式是整個公司的 \- 企業資源規劃 \( ERP \) 應用程式。 ERP 應用程式裝載于多部執行 Windows Server 2016 的電腦上。 在 Active Directory Domain Services 中，這些電腦是 \( \) 針對企業營運 \( LOB \)  應用程式伺服器所建立之組織單位 OU 的成員。 \-ERP 應用程式的用戶端元件會安裝在執行 Windows 10 和 Windows 8.1 的電腦上。

在群組原則中，IT 系統管理員會選取將套用 QoS 原則的群組原則物件 \( GPO \) 。 藉由使用 QoS 原則嚮導，IT 系統管理員會建立名為「伺服器 LOB 原則」的 QoS 原則， \- 為所有應用程式、任何 IP 位址、TCP 和 UDP 和埠號碼指定高優先順序的 DSCP 值44。

QoS 原則只會透過群組原則管理主控台 GPMC 工具，將 GPO 連結到僅包含這些伺服器的 OU，以套用至 LOB 服務器 \( \) 。 當電腦傳送網路流量時，此初始伺服器 LOB 原則會套用高 \- 優先順序的 DSCP 值。 您稍後可以 \( 在群組原則物件編輯器工具中編輯此 QoS 原則 \) ，以包含 ERP 應用程式的埠號碼，這會限制只有在使用指定的埠號碼時才能套用原則。

#### <a name="qos-policy-for-the-finance-group"></a>財務群組的 QoS 原則

雖然公司內的許多群組都存取 ERP 應用程式，但財務部門在處理客戶時相依于此應用程式，而該群組需要應用程式的一致效能。

為確保財務群組可支援其客戶，QoS 原則必須將這些使用者的流量分類為高優先順序。 不過，當財務群組的成員使用 ERP 應用程式以外的應用程式時，原則應該不適用。

因此，IT 部門在群組原則物件編輯器工具中定義稱為「用戶端 LOB 原則」的第二個 QoS 原則，當財務使用者群組執行 ERP 應用程式時，就會套用 DSCP 值60。

#### <a name="qos-policy-for-a-backup-app"></a>備份應用程式的 QoS 原則

在所有電腦上都有個別的備份應用程式正在執行。 為確保備份應用程式的流量不會使用所有可用的網路資源，IT 部門會建立備份資料原則。 這項備份原則會根據備份應用程式的可執行檔名稱（ **backup.exe**）來指定 DSCP 值1。

為網域中的所有用戶端電腦建立並部署第三個 GPO。 當備份應用程式傳送資料時，即使是來自財務部門的電腦，也會套用低優先順序的 DSCP 值。

>[!NOTE]
>沒有 QoS 原則的網路流量會以 DSCP 值0傳送。

### <a name="scenario-policies"></a>案例原則

下表摘要說明此案例的 QoS 原則。

|原則名稱|DSCP 值|節流速率|適用于組織單位|描述|
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[沒有原則]|0|無|[無部署]|針對未分類的流量， (預設) 處理工作的最佳做法。|
|備份資料|1|無|所有用戶端|針對此大量資料套用低優先順序的 DSCP 值。|
|伺服器 LOB|44|無|ERP 伺服器的電腦 OU|適用于 ERP 伺服器流量的高優先順序 DSCP|
|用戶端 LOB|60|無|財務使用者群組|適用于 ERP 用戶端流量的高優先順序 DSCP|

>[!NOTE]
>DSCP 值會以十進位形式表示。

使用群組原則定義並套用 QoS 原則後，輸出網路流量會接收原則指定的 DSCP 值。 然後，路由器會使用佇列，根據這些 DSCP 值提供差異處理。 針對這個 IT 部門，路由器設定有四個佇列：高優先順序、中間優先順序、最佳工作和低優先順序。

當流量抵達具有「伺服器 LOB 原則」和「用戶端 LOB 原則」 DSCP 值的路由器時，資料會放入高優先順序的佇列。 DSCP 值為0的流量會獲得最大的服務等級。 從備份應用程式取得 DSCP 值 1 (的封包，) 接收低優先順序的處理。

### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>排定企業營運應用程式優先順序的必要條件

若要完成這項工作，請確定您符合下列需求：

- 相關電腦正在執行 QoS \- 相容的作業系統。

- 相關電腦是 Active Directory Domain Services AD DS 網域的成員 \( ， \) 因此可以使用群組原則進行設定。

- TCP/IP 網路的設定是設定為 DSCP RFC 2474 的路由器 \( \) 。 如需詳細資訊，請參閱 [RFC 2474](https://www.ietf.org/rfc/rfc2474.txt)。

- 符合系統管理認證需求。

#### <a name="administrative-credentials"></a>系統管理認證

若要完成這項工作，您必須能夠建立和部署群組原則物件。

#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>設定測試環境以排列企業營運應用程式的優先順序

若要設定測試環境，請完成下列工作。

- 使用群組至組織單位的用戶端和使用者建立 AD DS 網域。 如需部署 AD DS 的指示，請參閱《 [核心網路指南》](../../core-network-guide/core-network-guide.md)。

- 根據 DSCP 值設定路由器以微分佇列。 例如，DSCP 值44會進入「白金」佇列，而所有其他佇列則會以加權的佇列。

>[!NOTE]
> 您可以使用網路捕獲搭配網路監視器之類的工具來查看 DSCP 值。 執行網路抓取之後，您可以觀察到已捕捉的資料中的 TOS 欄位。

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>針對企業營運應用程式排列優先順序的步驟

若要設定企業營運應用程式的優先順序，請完成下列工作：

1. 建立群組原則物件 GPO 並連結 \( 到 \) QoS 原則。

2. 根據選取的 DSCP 值，設定路由器以微分將企業營運應用程式 (視為使用佇列) 。 這項工作的程式會根據您擁有的路由器類型而有所不同。

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>案例2：設定 HTTP 伺服器應用程式網路流量的優先順序

在 Windows Server 2016 中，以原則為基礎的 QoS 包含以 URL 為基礎的功能。 URL 原則可讓您管理 HTTP 伺服器的頻寬。

許多企業應用程式都是針對 Internet Information Services 的 IIS web 伺服器進行開發和裝載 \( \) ，而 web 應用程式則是從用戶端電腦上的瀏覽器存取。

在此案例中，假設您管理一組 IIS 伺服器，這些伺服器會為您組織的所有員工託管訓練影片。 您的目標是要確保來自這些影片伺服器的流量不會讓您的網路負荷嚴重，並確保影片流量與網路上的語音和資料流量有所區別。

工作類似于案例1中的工作。 您將會設計及設定流量管理設定，例如影片流量的 DSCP 值，以及與企業營運應用程式相同的節流速率。 但在指定流量時，您只需要輸入 HTTP 伺服器應用程式將回應的 URL，而不是提供應用程式名稱：例如， https://hrweb/training 。

> [!NOTE]
>您無法使用以 URL 為基礎的 QoS 原則，為執行 windows 7 和 Windows Server 2008 R2 之前發行之 Windows 作業系統的電腦設定網路流量的優先順序。

### <a name="precedence-rules-for-url-based-policies"></a>以 URL 為基礎的原則的優先順序規則

下列所有 Url 都有效，而且可以在 QoS 原則中指定並同時套用到電腦或使用者：

- https://video

- https://training.hr.mycompany.com

- https://10.1.10.249:8080/tech

- HTTPs：//*/ebooks

但哪一個會獲得優先權？ 規則很簡單。 以 URL 為基礎的原則會以由左至右的讀取順序排列優先順序。 因此，從最高優先順序到最低優先順序，URL 欄位如下：

[1. URL 配置](#bkmk_QoS_UrlScheme)

[2. URL 主機](#bkmk_QoS_UrlHost)

[3. URL 埠](#bkmk_QoS_UrlPort)

[4. URL 路徑](#bkmk_QoS_UrlPath)

詳細資料如下:

####  <a name="1-url-scheme"></a><a name="bkmk_QoS_UrlScheme"></a> 1. URL 配置

 `https://` 的優先順序高於 `https://` 。

####  <a name="2-url-host"></a><a name="bkmk_QoS_UrlHost"></a> 2. URL 主機

 從最高優先順序到最低，它們是：

1. Hostname (主機名稱)

2. IPv6 位址

3. IPv4 位址

4. 萬用字元

在主機名稱的案例中，有更多點素的主機名稱 (更深度) 具有比具有較少點元素的主機名稱更高的優先權。 例如，在下列主機名稱中：

- video.internal.training.hr.mycompany.com (depth = 6) 

- selfguide.training.mycompany.com (depth = 4) 

- 訓練 (深度 = 1) 

- 程式庫 (深度 = 1) 

  **video.internal.training.hr.mycompany.com** 具有最高優先順序，而 **selfguide.training.mycompany.com** 具有下一個最高優先順序。 **定型** 和連結 **庫** 共用相同的最低優先順序。

####  <a name="3-url-port"></a><a name="bkmk_QoS_UrlPort"></a> 3. URL 埠

特定或隱含的埠號碼的優先順序高於萬用字元埠。

####  <a name="4-url-path"></a><a name="bkmk_QoS_UrlPath"></a> 4. URL 路徑

如同主機名稱，URL 路徑可能包含多個元素。 具有更多元素的專案，其優先順序一律高於較少的專案。 例如，下列路徑依優先順序列出：

1.  /ebooks/tech/windows/networking/qos

2.  /ebooks/tech/windows/

3.  /ebooks

4.  /

如果使用者選擇包含 URL 路徑之後的所有子目錄和檔案，則此 URL 路徑的優先順序將會比未進行選擇時的優先順序低。

使用者也可以選擇在以 URL 為基礎的原則中指定目的地 IP 位址。 目的地 IP 位址的優先順序低於先前所述的四個 URL 欄位中的任何一個。

### <a name="quintuple-policy"></a>Quintuple 原則

Quintuple 原則是依通訊協定識別碼、來源 IP 位址、來源埠、目的地 IP 位址和目的地埠來指定。 Quintuple 原則的優先順序一律高於任何以 URL 為基礎的原則。

如果已對使用者套用 Quintuple 原則，則新的 URL 型原則將不會在該使用者的任何用戶端電腦上造成衝突。

如需本指南的下一個主題，請參閱 [管理 QoS 原則](qos-policy-manage.md)。

本指南的第一個主題中，請參閱 [服務品質 (QoS) 原則](qos-policy-top.md)。
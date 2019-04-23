---
title: QoS 原則案例
description: 本主題提供的服務品質 (QoS) 原則案例，示範如何使用群組原則來設定特定的應用程式和服務 Windows Server 2016 中的網路流量的優先順序。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2a24ce3213b1aa1c66a438d278f7499b6d53bf6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834349"
---
# <a name="qos-policy-scenarios"></a>QoS 原則案例

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來檢閱假設性的案例，用來示範如何、 何時以及為何来使用 QoS 原則。

本主題中的兩個案例包括：

1. 特定業務應用程式排列網路流量的優先順序
2. HTTP 伺服器應用程式排列網路流量的優先順序

>[!NOTE]
>本主題的某些小節包含執行所述的動作時，您可以採取的一般步驟。 如需詳細指示管理 QoS 原則，請參閱 <<c0> [ 管理 QoS 原則](qos-policy-manage.md)。

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>案例 1：特定業務應用程式排列網路流量的優先順序

在此案例中，IT 部門會有數個目標，他們可以使用 QoS 原則完成：

- 提供較佳的網路效能的關鍵\-關鍵應用程式。
- 在使用特定的應用程式時，請提供更佳的索引鍵的一份使用者的網路效能。
- 請確認該公司\-寬資料備份應用程式進行一次使用太多頻寬，不會妨礙某些網路效能。

IT 部門決定，來設定 QoS 原則，以設定特定的應用程式的使用差異服務字碼指標\(DSCP\)值來分類網路流量，並將其提供優先的路由器設定較高的優先順序流量的處理方式。 

>[!NOTE]
>如需有關 DSCP 的詳細資訊，請參閱下節**定義 QoS 優先順序透過差異服務字碼指標**主題中的 <<c4> [ 服務品質 (QoS) 原則](qos-policy-top.md)。

DSCP 值，除了 QoS 原則可以指定節流閥速率。 節流的效果的所有的輸出流量的相符項目在特定的 QoS 原則的傳送速率限制。

### <a name="qos-policy-configuration"></a>QoS 原則設定

若要完成三個個別的目標，IT 系統管理員決定建立三個不同的 QoS 原則。

#### <a name="qos-policy-for-lob-app-servers"></a>LOB 應用程式伺服器的 QoS 原則

第一個任務\-重要應用程式，IT 部門建立 QoS 原則是一家公司\-寬企業資源規劃\(ERP\)應用程式。 ERP 應用程式被裝載在數個全都執行 Windows Server 2016 的電腦上。 在 Active Directory 網域服務，這些電腦屬於組織單位\(OU\)所建立的特定業務\(LOB\)應用程式伺服器。 用戶端\-的 ERP 應用程式的側邊元件安裝在執行 Windows 10 和 Windows 8.1 的電腦上。

在群組原則中，IT 系統管理員選取 群組原則物件\(GPO\)時套用 QoS 原則。 使用 QoS 原則精靈，IT 系統管理員建立 QoS 原則，稱為 「 LOB 伺服器原則 」，指定較高\-優先順序 DSCP 值 44 的所有應用程式、 任何 IP 位址、 TCP 和 UDP 和連接埠號碼。

QoS 原則只會套用到 LOB 伺服器藉由將 GPO 連結到 OU，其中包含只有這些伺服器，透過群組原則管理主控台\(GPMC\)工具。 此初始伺服器 LOB 原則適用於高\-優先順序 DSCP 值，每當電腦傳送網路流量。 此 QoS 原則可以稍後編輯\(「 群組原則物件編輯器 」 工具中\)包含 ERP 應用程式的連接埠號碼，這會限制要套用指定的連接埠號碼在使用時才的原則。

#### <a name="qos-policy-for-the-finance-group"></a>財務群組的 QoS 原則

而在公司內的多個群組存取 ERP 應用程式，財務群組相依於此應用程式的客戶，在處理時，群組需要從應用程式一直都很高的效能。

若要確保的 finance 群組可支援其客戶，QoS 原則必須將這些使用者的流量分類為高優先順序。 不過，財務群組的成員使用的 ERP 應用程式以外的應用程式時，應該不套用原則。 

因為這個緣故，IT 部門會定義第二個 QoS 原則，稱為 「 用戶端 LOB 原則 」 中適用於 DSCP 值為 60，財務使用者群組執行 ERP 應用程式時的群組原則物件編輯器工具。

#### <a name="qos-policy-for-a-backup-app"></a>備份應用程式的 QoS 原則

在所有電腦上執行個別的備份應用程式。 若要確保備份的應用程式流量不會使用所有可用的網路資源，IT 部門建立備份的資料原則。 此備份原則指定 DSCP 值為 1 以備份應用程式中，也就是可執行檔的名稱為基礎**backup.exe**。 

第三個 GPO 建立並部署在網域中的所有用戶端電腦。 每當備份的應用程式傳送資料，會套用的低優先順序的 DSCP 值，即使它來自財務部門中的電腦。
  
>[!NOTE]
>如果未使用 QoS 原則的網路流量會傳送 DSCP 值是 0。

### <a name="scenario-policies"></a>案例原則

下表摘要說明此案例中的 QoS 原則。
  
|原則名稱|DSCP 值|節流閥速率|套用至組織單位|描述|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[沒有原則]|0|None|[不部署]|未分類的流量的最佳投入時間 （預設值） 處理。|  
|備份資料|1|None|所有用戶端|適用於低優先順序的 DSCP 值，此大量資料。|  
|LOB 伺服器|44|None|電腦 OU ERP 伺服器|適用於高優先順序的 ERP 伺服器流量的 DSCP|  
|用戶端 LOB|60|None|財務使用者群組|適用於高優先順序的 ERP 用戶端流量的 DSCP|  

>[!NOTE]
>DSCP 值會以小數格式表示。

使用 QoS 原則定義和使用群組原則套用，輸出網路流量會接收原則指定 DSCP 值。 路由器則提供使用佇列，根據這些 DSCP 值的處理方式差異。 使用四個佇列設定路由器為本的 IT 部門，： 高優先順序、 中間優先、 最佳方式和低優先順序。

當流量到達路由器使用 DSCP 值，從"LOB 伺服器原則 」 和 「 用戶端 LOB 原則，"資料放入高優先順序佇列。 流量的 DSCP 值是 0，會收到的最佳服務等級。 封包的 DSCP 值為 1 （從備份的應用程式） 接收低優先順序的處理方式。  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>排列優先順序的特定業務應用程式的必要條件

若要完成這項工作，請確定您符合下列需求：

- 涉及的電腦執行 QoS\-相容的作業系統。

- 涉及的電腦是 Active Directory 網域服務的成員\(AD DS\)網域，讓它們可以使用群組原則設定。

- TCP/IP 網路設定與設定 DSCP 的路由器\(RFC 2474\)。 如需詳細資訊，請參閱 < [RFC 2474](https://www.ietf.org/rfc/rfc2474.txt)。

- 符合系統管理認證需求。

#### <a name="administrative-credentials"></a>系統管理認證

若要完成這項工作中，您必須能夠建立和部署群組原則物件。
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>設定測試環境，排列優先順序的特定業務應用程式

若要設定測試環境，請完成下列工作。

- 建立 AD DS 網域與用戶端及使用者分組到組織單位。 如需部署 AD DS 的指示，請參閱 <<c0> [ 核心網路指南](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide)。

- 設定路由器，微分 DSCP 值為基礎的佇列。 比方說，DSCP 值 44 進入 「 白金 」 佇列，以及所有其他則加權-公平佇列。

>[!NOTE]
> 您可以使用網路監視器之類的工具中的網路擷取，以檢視 DSCP 值。 當您執行網路擷取之後，您可以觀察 TOS 欄位中擷取的資料。

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>排列優先順序的特定業務應用程式的步驟

若要設定優先順序的特定業務應用程式，完成下列工作：

1. 建立並連結群組原則物件\(GPO\)與 QoS 原則。

2. 設定路由器，微分處理的特定業務應用程式 （透過使用佇列） 會根據選取的 DSCP 值。 這項工作的程序會有所不同的路由器，您有型別。

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>案例 2：HTTP 伺服器應用程式排列網路流量的優先順序

在 Windows Server 2016 中，原則為依據的 QoS 會包含功能的 URL 為基礎的原則。 URL 原則可讓您管理頻寬的 HTTP 伺服器。

許多企業應用程式是針對開發和 Internet Information Services 上裝載\(IIS\)從用戶端電腦上的瀏覽器存取 web 伺服器和 Web 應用程式。

在此案例中，假設您管理 IIS 伺服器的一組該主機訓練影片為您組織的所有員工。 您的目標是為了確保這些影片的伺服器的流量不會拖垮網路，並確定 視訊流量區別語音及資料網路上的流量。 

工作是與案例 1 中的工作類似。 您將設計與設定的流量管理設定，例如視訊流量的 DSCP 值和節流速率相同像您一樣的特定業務應用程式。 但在指定的流量，而不是提供應用程式名稱，您只輸入 HTTP 伺服器應用程式會回應的 URL： 例如， https://hrweb/training。
  
> [!NOTE]
>您無法使用 URL 為依據的 QoS 原則來設定執行 Windows 7 和 Windows Server 2008 R2 之前發行的 Windows 作業系統之電腦的網路流量的優先順序。

### <a name="precedence-rules-for-url-based-policies"></a>優先順序規則的 URL 為基礎的原則

所有下列 Url 有效，而且可以是 QoS 原則中指定，並同時套用至電腦或使用者：

- https://video

- https://training.hr.mycompany.com

- https://10.1.10.249:8080/tech  

- https://*/ebooks

但是，其中將會收到優先順序？ 規則很簡單。 URL 為基礎的原則是由左到右的讀取順序排列優先順序。 因此，從最高的優先順序到最低優先順序 URL 欄位如下：
  
[1.URL 配置](#bkmk_QoS_UrlScheme)

[2.URL 主機](#bkmk_QoS_UrlHost)

[3.URL 的連接埠](#bkmk_QoS_UrlPort)

[4.URL 路徑](#bkmk_QoS_UrlPath)

詳細資料如下所示：

####  <a name="bkmk_QoS_UrlScheme"></a> 1.URL 配置

 `https://` 具有較高的優先順序高於`https://`。

####  <a name="bkmk_QoS_UrlHost"></a> 2.URL 主機

 從最高的優先順序到最低，它們是：

1. 主機名稱

2. IPv6 位址

3. IPv4 位址

4. 萬用字元

如果主機名稱，是與多點分隔的項目 （更深入的討論） 的主機名稱會具有較高的優先順序比具有較少的點狀元素的主機名稱。 例如，在下列的主機名稱：

- video.internal.training.hr.mycompany.com (depth = 6)
  
- selfguide.training.mycompany.com (depth = 4)
  
- 訓練 (深度 = 1)
  
- 程式庫 (深度 = 1)
  
 **video.internal.training.hr.mycompany.com**具有最高的優先權，並**selfguide.training.mycompany.com**有下一個最高的優先順序。 **定型**並**程式庫**共用相同的最低優先順序。  
  
####  <a name="bkmk_QoS_UrlPort"></a> 3.URL 的連接埠

特定或隱含的連接埠的數字具有較高的優先順序高於萬用字元連接埠。

####  <a name="bkmk_QoS_UrlPath"></a> 4.URL 路徑

主機名稱，例如 URL 路徑可能包含多個項目。 具有多個項目一律會有較高的優先順序比具有較少。 例如，下列路徑會依照優先順序列出：  

1.  /ebooks/tech/windows/networking/qos

2.  /ebooks/tech/windows/

3.  /ebooks

4.  /

如果使用者選擇要包含所有子目錄和下列 URL 路徑的檔案時，此 URL 路徑會有較低的優先順序比它會有未設為選擇。

使用者也可以選擇以 URL 為基礎的原則中指定的目的地 IP 位址。 目的地 IP 位址具有較低的優先順序，高於任何先前所述的四個 URL 欄位。
  
### <a name="quintuple-policy"></a>5 倍的原則

5 倍的原則是由通訊協定識別碼、 來源 IP 位址、 來源連接埠、 目的地 IP 位址和目的地連接埠指定。 5 倍的原則一律會有較高的優先順序，高於任何 URL 為基礎的原則。 

如果使用者已經套用了 5 倍的原則，以新的 URL 為基礎原則不會造成衝突任何該使用者的用戶端電腦。

如本指南中的下一個主題，請參閱 <<c0> [ 管理 QoS 原則](qos-policy-manage.md)。

如本指南中的第一個主題，請參閱 <<c0> [ 服務品質 (QoS) 原則](qos-policy-top.md)。

---
title: RADIUS 用戶端
description: 本主題提供 Windows Server 2016 中的網路原則伺服器的 RADIUS 用戶端的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fdca45237d971c1b2e08443112b63d3ce77e4a2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874309"
---
# <a name="radius-clients"></a>RADIUS 用戶端

>適用於：Windows Server （半年通道），Windows Server 2016

網路存取伺服器\(NAS\)是提供某種程度的大型網路存取權的裝置。 使用 RADIUS 基礎結構的 NAS 也是 RADIUS 用戶端，傳送連接要求及帳戶處理訊息到 RADIUS 伺服器進行驗證、 授權以及帳戶處理。

>[!NOTE]
>用戶端電腦，例如膝上型電腦及其他執行用戶端作業系統的電腦不是 RADIUS 用戶端。 RADIUS 用戶端網路存取伺服器-例如無線存取點、 802.1 X 驗證交換器、 虛擬私人網路\(VPN\)伺服器和撥號伺服器-因為它們使用 RADIUS 通訊協定來與 RADIUS 通訊網路原則伺服器的伺服器\(NPS\)伺服器。

若要部署 NPS 為 RADIUS 伺服器或 RADIUS proxy，您必須在 NPS 中設定 RADIUS 用戶端。

## <a name="radius-client-examples"></a>RADIUS 用戶端範例

網路存取伺服器的範例如下：

- 提供組織網路或網際網路的遠端存取連線的網路存取伺服器。 舉例來說，組織內部網路中執行 Windows Server 2016 作業系統和遠端存取服務的提供是傳統撥號或虛擬私人網路 (VPN) 遠端存取服務的電腦。
- 提供使用無線為基礎的傳輸與接收技術的組織網路實體層存取的無線存取點。
- 提供使用傳統 LAN 技術，例如乙太網路的組織網路實體層存取的參數。
- RADIUS proxy，將連線要求轉送到 RADIUS 伺服器的 RADIUS proxy 設定的遠端 RADIUS 伺服器群組的成員。

## <a name="radius-access-request-messages"></a>RADIUS Access-request 訊息

RADIUS 用戶端建立 RADIUS Access-request 訊息，並將其轉送到 RADIUS proxy 或 RADIUS 伺服器，或者將 Access-request 訊息到 RADIUS 伺服器，它們來自另一個 RADIUS 用戶端都已接收但本身尚未建立。

RADIUS 用戶端不會執行驗證、 授權以及帳戶處理所處理 Access-request 訊息。 只有 RADIUS 伺服器會執行這些函式。

NPS 中，不過，可以設定為 RADIUS proxy 與 RADIUS 伺服器，同時，讓它處理一些 Access-request 訊息及轉送其他訊息。

## <a name="nps-as-a-radius-client"></a>NPS 做為 RADIUS 用戶端

NPS 會扮演 RADIUS 用戶端，當您將它設定為 RADIUS proxy 將 Access-request 訊息轉送到其他 RADIUS 伺服器進行處理。 當您使用 NPS 做為 RADIUS proxy 時，下列的一般組態步驟所需：

1. 網路存取伺服器，例如無線存取點與 VPN 伺服器，設定 NPS proxy 做為指定的 RADIUS 伺服器或驗證伺服器的 IP 位址。 這可讓網路存取伺服器，建立 Access-request 訊息根據資訊它們從存取用戶端，將訊息轉送到 NPS proxy 接收。

2. 藉由新增每個網路存取伺服器的 RADIUS 用戶端設定 NPS proxy。 此組態步驟可讓 NPS proxy 從網路存取伺服器接收訊息，並在驗證期間與它們通訊。 此外，會設定 NPS proxy 上的連線要求原則，以指定哪些 Access-request 訊息轉送到一或多個 RADIUS 伺服器。 這些原則也會設定使用遠端 RADIUS 伺服器群組，告訴 NPS 將從網路存取伺服器接收的訊息傳送到何處。

3. NPS 或其他 RADIUS 伺服器，NPS proxy 上遠端 RADIUS 伺服器群組的成員會設定為從 NPS proxy 接收訊息。 這是藉由設定 NPS proxy 做為 RADIUS 用戶端來完成。

## <a name="radius-client-properties"></a>RADIUS 用戶端內容

當您新增 RADIUS 用戶端至 NPS 設定透過 NPS 主控台，或透過使用 NPS 的 netsh 命令或 Windows PowerShell 命令時，您要設定 NPS 從網路存取伺服器接收 RADIUS Access-request 訊息或RADIUS proxy。

當您在 NPS 中設定 RADIUS 用戶端時，您可以指定下列屬性：

### <a name="client-name"></a>用戶端名稱

 RADIUS 用戶端，可讓您更輕鬆地找出 nps 使用 NPS 嵌入式管理單元或 netsh 命令時的易記名稱。

### <a name="ip-address"></a>IP 位址

網際網路通訊協定第 4 版\(IPv4\)位址或網域名稱系統\(DNS\) RADIUS 用戶端的名稱。

### <a name="client-vendor"></a>用戶端廠商

RADIUS 用戶端的廠商。 否則，您可以使用 RADIUS 標準值的用戶端廠商。

### <a name="shared-secret"></a>共用密碼

做為 RADIUS 用戶端、 RADIUS 伺服器與 RADIUS proxy 之間密碼文字字串。 使用訊息驗證者屬性時，共用的密碼也會做為金鑰來加密 RADIUS 訊息。 此字串必須設定 RADIUS 用戶端與 NPS 嵌入式管理單元。

### <a name="message-authenticator-attribute"></a>訊息驗證者屬性

RFC 2869，「 RADIUS 擴充功能，"Message Digest 5 中所述\(MD5\)整個 RADIUS 訊息的雜湊。 如果 RADIUS 訊息驗證者屬性，則它會驗證它。 如果驗證失敗，則會捨棄 RADIUS 訊息。 如果用戶端設定需要訊息驗證者屬性，並不存在，則會捨棄 RADIUS 訊息。 建議使用訊息驗證者屬性。

>[!NOTE]
>就需要訊息驗證者屬性，而且依預設啟用，當您使用可延伸驗證通訊協定\(EAP\)驗證。 

如需 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS)](nps-top.md)。


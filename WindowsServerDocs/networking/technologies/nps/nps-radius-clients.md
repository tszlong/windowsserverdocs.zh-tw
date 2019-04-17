---
title: RADIUS 戶端
description: 本主題提供適用於 Windows Server 2016 中的網路原則伺服器的 RADIUS 用概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a970dabcc6f3f4fc4d8ed5a0dedd01b5a7dee71a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="radius-clients"></a>RADIUS 戶端

>適用於：Windows Server（以每年次管道）、Windows Server 2016

網路存取伺服器 \(NAS\) 是提供更大網路的存取權的一些層級的裝置。 使用介紹 NAS 也會傳送連接要求和計量訊息 RADIUS 伺服器的驗證，驗證，以及計量 RADIUS client。

>[!NOTE]
>Client 電腦、膝上型電腦和其他執行 client 作業系統的電腦不是 RADIUS 戶端。 RADIUS 戶端而網路存取伺服器-例如 wireless 存取點，802.1 X 驗證的參數，virtual 私人網路 \(VPN\) 伺服器撥號伺服器-因為它們可以使用 RADIUS 通訊協定進行通訊的網路原則伺服器 \(NPS\) 伺服器例如 RADIUS 伺服器。

若要部署 NPS RADIUS 伺服器或 RADIUS proxy，您必須設定 NPS RADIUS 戶端。

## <a name="radius-client-examples"></a>RADIUS client 範例

網路存取伺服器的範例包括：

- 提供遠端存取連接到組織網路或網際網路的網路存取伺服器。 一個範例是執行 Windows Server 2016 作業系統和遠端存取服務，可提供「傳統撥號或 virtual 私人網路 (VPN) 遠端存取服務與組織內部網路的電腦。
- Wireless 存取點，提供層實體網路的存取權的組織使用無線傳送和接收技術。
- 提供實體層組織的網路，使用傳統區域網路技術，例如乙太網路的存取權的參數。
- RADIUS proxy 轉送連接要求 RADIUS 伺服器 RADIUS proxy 設定遠端 RADIUS 伺服器群組成員。

## <a name="radius-access-request-messages"></a>RADIUS 存取要求訊息

RADIUS 戶端建立 RADIUS 存取要求訊息和轉寄 RADIUS proxy 或 RADIUS 伺服器，或他們轉寄要求存取 RADIUS 伺服器收到從另一位 RADIUS 用，但無法建立自己的訊息。

RADIUS 戶端不執行驗證，驗證，並計量處理要求存取訊息。 只有 RADIUS 伺服器執行這些功能。

NPS，但是，可以設定為 RADIUS proxy 和 RADIUS 伺服器同時，處理一些要求存取訊息，使其轉送其他訊息。

## <a name="nps-as-a-radius-client"></a>NPS RADIUS client 為

當您將其設定為 [轉寄存取要求訊息，以處理的其他 RADIUS 伺服器 RADIUS proxy NPS 作為 RADIUS client。 當您使用 NPS RADIUS proxy 時，所一般設定下列步驟：

1. Wireless 存取點和 VPN 伺服器的網路存取伺服器具有 NPS proxy 指定的 RADIUS 伺服器或驗證伺服器的 IP 位址設定。 這可讓建立要求存取訊息依據他們會從存取，來將郵件轉寄給 NPS proxy 收到網路存取伺服器。

2. NPS proxy 設定來新增為 RADIUS client 的每個網路的存取伺服器。 這項設定步驟可讓您從網路存取伺服器接收簡訊並與他們在驗證期間 NPS proxy。 此外，在 NPS proxy 連接要求原則設定來指定存取要求郵件轉寄給一或多個 RADIUS 伺服器。 這些原則也會以遠端 RADIUS 伺服器群組，可將您的位置告知 NPS 將它從網路存取伺服器接收簡訊的位置設定。

3. NPS 或 NPS proxy 的遠端 RADIUS 伺服器群組成員其他 RADIUS 伺服器設定為從 NPS proxy 收到簡訊。 這是 NPS proxy RADIUS client 設定。

## <a name="radius-client-properties"></a>RADIUS client 屬性

當您新增 RADIUS client NPS 設定透過 NPS 主機或使用 netsh 命令 NPS 或 Windows PowerShell 命令時，您 NPS RADIUS 存取要求簡訊接收網路存取伺服器或 RADIUS proxy 設定。

當您設定 NPS RADIUS client 時，您可以指定下列屬性：

### <a name="client-name"></a>Client 名稱

 RADIUS client，讓它變得更容易使用 NPS NPS 嵌入式管理單元或 netsh 命令時，找出的易記名稱。

### <a name="ip-address"></a>IP 位址

網際網路通訊協定第 4 版 \(IPv4\) 地址或 RADIUS client 的網域名稱系統 \(DNS\) 名稱。

### <a name="client-vendor"></a>Client 廠商

RADIUS client 廠商。 否則，您可以使用 RADIUS 標準值為 Client 廠商。

### <a name="shared-secret"></a>共用的密碼

做為密碼 RADIUS 戶端、RADIUS 伺服器、和 RADIUS proxy 之間的文字。 當使用訊息 Authenticator 屬性時，共用的密碼也來做為鍵加密 RADIUS 訊息。 在 RADIUS client 和 NPS 嵌入式管理單元必須設定此字串。

### <a name="message-authenticator-attribute"></a>訊息 Authenticator 屬性

RFC 2869，「RADIUS 擴充功能」RADIUS 整個訊息的郵件摘要 5 \(MD5\) hash 所述。 如果有 RADIUS 訊息 Authenticator 屬性，它被確認。 驗證失敗時，會捨棄 RADIUS 訊息。 如果 client 設定需要訊息 Authenticator 屬性，並不存在，會捨棄 RADIUS 訊息。 建議使用的訊息 Authenticator 屬性。

>[!NOTE]
>訊息 Authenticator 屬性需要並使用延伸驗證通訊協定 \(EAP\) 驗證時，預設支援。 

如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。


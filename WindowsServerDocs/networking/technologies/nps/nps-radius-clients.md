---
title: RADIUS 用戶端
description: 本主題概要說明 Windows Server 2016 中網路原則伺服器的 RADIUS 用戶端。
manager: brianlic
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 2f664ad42781846ea44cb6b382b0078cbf403a4f
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947454"
---
# <a name="radius-clients"></a>RADIUS 用戶端

>適用於：Windows Server (半年度管道)、Windows Server 2016

網路存取伺服器 \( NAS \) 是一種裝置，可針對較大的網路提供某種層級的存取權。 使用 RADIUS 基礎結構的 NAS 也是 RADIUS 用戶端，傳送連線要求及帳戶處理訊息到 RADIUS 伺服器進行驗證、授權以及帳戶處理。

>[!NOTE]
>用戶端電腦（例如，膝上型電腦和其他執行用戶端作業系統的電腦）不是 RADIUS 用戶端。 RADIUS 用戶端是網路存取伺服器（例如無線存取點、802.1 X 驗證交換器、虛擬私人網路 \( VPN \) 伺服器和撥號伺服器），因為它們使用 radius 通訊協定與 radius 伺服器（例如網路原則伺服器 NPS 伺服器）通訊 \( \) 。

若要將 NPS 部署為 RADIUS 伺服器或 RADIUS proxy，您必須在 NPS 中設定 RADIUS 用戶端。

## <a name="radius-client-examples"></a>RADIUS 用戶端範例

網路存取伺服器的範例包括：

- 提供組織網路或網際網路的遠端存取連線能力的網路存取伺服器。 例如，執行 Windows Server 2016 作業系統和遠端存取服務的電腦，會提供傳統的撥號或虛擬私人網路 (VPN) 遠端存取服務給組織內部網路。
- 提供組織網路實體層存取的無線存取點，使用以無線為基礎的傳輸與接收技術。
- 提供組織網路實體層存取的交換器，使用傳統的 LAN 技術，例如乙太網路。
- 將連線要求轉送至屬於遠端 RADIUS 伺服器群組成員的 RADIUS 伺服器的 RADIUS Proxy，此伺服器群組設定在 RADIUS Proxy 上。

## <a name="radius-access-request-messages"></a>RADIUS Access-Request 訊息

RADIUS 用戶端建立 RADIUS Access-Request 訊息，並將它們轉送到 RADIUS Proxy 或 RADIUS 伺服器；或者將 Access-Request 訊息轉送到已經從另一個 RADIUS 用戶端接收但本身尚未建立的 RADIUS 伺服器。

RADIUS 用戶端不會藉由執行驗證、授權以及帳戶處理來處理 Access-Request 訊息。 只有 RADIUS 伺服器會執行這些功能。

不過，可以將 NPS 同時設定為 RADIUS Proxy 及 RADIUS 伺服器，使其可處理一些 Access-Request 訊息及轉送其他訊息。

## <a name="nps-as-a-radius-client"></a>NPS 做為 RADIUS 用戶端

當您將 NPS 設定為 RADIUS Proxy 以便將 Access-Request 訊息轉送到其他 RADIUS 伺服器進行處理時，NPS 將扮演 RADIUS 用戶端。 當您使用 NPS 做為 RADIUS Proxy 時，需要下列一般設定步驟：

1. 使用 NPS Proxy 的 IP 位址將網路存取伺服器 (例如無線存取點與 VPN 伺服器) 設定為指定的 RADIUS 伺服器或驗證伺服器。 這個設定可讓網路存取伺服器 (根據它們從存取用戶端接收的資訊建立 Access-Request 訊息) 將訊息轉送到 NPS Proxy。

2. 藉由新增每個網路存取伺服器做為 RADIUS 用戶端，設定 NPS Proxy。 此設定步驟可讓 NPS Proxy 從網路存取伺服器接收訊息，然後在驗證期間與它們通訊。 此外，設定 NPS Proxy 上的連線要求原則，指定哪些 Access-Request 訊息要轉送到一或多部 RADIUS 伺服器。 您也可以使用遠端 RADIUS 伺服器群組設定這些原則，告訴 NPS 將從網路存取伺服器接收的訊息傳送到何處。

3. 設定屬於 NPS Proxy 上遠端 RADIUS 伺服器群組成員的 NPS 或其他 RADIUS 伺服器，從 NPS Proxy 接收訊息。 藉由設定 NPS Proxy 為 RADIUS 用戶端即可完成此動作。

## <a name="radius-client-properties"></a>RADIUS 用戶端內容

當您透過 NPS 主控台將 RADIUS 用戶端新增至 NPS 設定，或使用 NPS 或 Windows PowerShell 命令的 netsh 命令時，您會將 NPS 設定為接收來自網路存取伺服器或 RADIUS proxy 的 RADIUS Access-Request 訊息。

當您在 NPS 中設定 RADIUS 用戶端時，可以指定下列內容：

### <a name="client-name"></a>用戶端名稱

 RADIUS 用戶端的好記名稱，使其在使用 NPS 嵌入式管理單元或 NPS 的 netsh 命令時易於識別。

### <a name="ip-address"></a>IP 位址

\( \) RADIUS 用戶端的第四版網際網路協定 IPv4 位址或網域名稱系統 \( DNS \) 名稱。

### <a name="client-vendor"></a>Client-Vendor

RADIUS 用戶端的廠商。 也可以使用 Client-Vendor 的 RADIUS 標準值。

### <a name="shared-secret"></a>共用密碼

當作 RADIUS 用戶端、RADIUS 伺服器以及 RADIUS Proxy 之間密碼使用的文字字串。 使用 [訊息驗證者] 屬性時，共用密碼也當作加密 RADIUS 訊息的金鑰。 此字串必須在 RADIUS 用戶端及 NPS 嵌入式管理單元中設定。

### <a name="message-authenticator-attribute"></a>訊息驗證者屬性

描述于 RFC 2869 中的「RADIUS 延伸模組」，這是 \( 整個 RADIUS 訊息的訊息摘要 5 MD5 \) 雜湊。 如果有 RADIUS [訊息驗證者] 屬性，就會加以驗證。 如果驗證失敗，會捨棄 RADIUS 訊息。 如果用戶端設定需要 [訊息驗證者] 屬性，但該屬性不存在，會捨棄 RADIUS 訊息。 建議使用 [訊息驗證者] 屬性。

>[!NOTE]
>當您使用可延伸的驗證通訊協定 EAP 驗證時，預設需要和啟用訊息驗證者屬性 \( \) 。

如需有關 NPS 的詳細資訊，請參閱 [網路原則伺服器 (nps) ](nps-top.md)。


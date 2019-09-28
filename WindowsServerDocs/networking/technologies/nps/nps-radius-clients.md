---
title: RADIUS 用戶端
description: 本主題概要說明 Windows Server 2016 中網路原則伺服器的 RADIUS 用戶端。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1dfc1bb71d2800a8a9587e54147950dfd7fb371f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396003"
---
# <a name="radius-clients"></a>RADIUS 用戶端

>適用於：Windows Server (半年度管道)、Windows Server 2016

網路存取伺服器 \(NAS @ no__t-1 是提供較大型網路存取層級的裝置。 使用 RADIUS 基礎結構的 NAS 也是 RADIUS 用戶端，將連線要求和帳戶處理訊息傳送至 RADIUS 伺服器，以進行驗證、授權和帳戶處理。

>[!NOTE]
>用戶端電腦（例如膝上型電腦和其他執行用戶端作業系統的電腦）不是 RADIUS 用戶端。 RADIUS 用戶端是網路存取伺服器，例如無線存取點、802.1 X 驗證交換器、虛擬私人網路 \(VPN @ no__t-1 部伺服器和撥號伺服器，因為它們使用 RADIUS 通訊協定與 RADIUS 伺服器（例如做為網路原則伺服器 \(NPS @ no__t-3 部伺服器。

若要將 NPS 部署為 RADIUS 伺服器或 RADIUS proxy，您必須在 NPS 中設定 RADIUS 用戶端。

## <a name="radius-client-examples"></a>RADIUS 用戶端範例

網路存取伺服器的範例如下：

- 為組織網路或網際網路提供遠端存取連線的網路存取伺服器。 例如，執行 Windows Server 2016 作業系統的電腦和遠端存取服務，可提供傳統的撥號或虛擬私人網路（VPN）遠端存取服務給組織內部網路。
- 無線存取點，使用以無線為基礎的傳輸與接收技術，為組織網路提供實體層存取。
- 使用傳統 LAN 技術（例如 Ethernet）提供組織網路之實體層存取的交換器。
- 將連線要求轉寄到 radius 伺服器（在 RADIUS proxy 上設定的遠端 RADIUS 伺服器群組的成員）的 RADIUS 代理程式。

## <a name="radius-access-request-messages"></a>RADIUS 存取要求訊息

RADIUS 用戶端會建立 RADIUS 存取要求訊息，並將它們轉送到 RADIUS proxy 或 RADIUS 伺服器，或將存取要求訊息轉送到已從其他 RADIUS 用戶端接收但尚未自行建立的 RADIUS 伺服器。

RADIUS 用戶端不會藉由執行驗證、授權和帳戶管理來處理存取要求訊息。 只有 RADIUS 伺服器會執行這些功能。

不過，NPS 可以同時設定為 RADIUS proxy 和 RADIUS 伺服器，以便處理一些存取要求訊息並轉送其他訊息。

## <a name="nps-as-a-radius-client"></a>NPS 做為 RADIUS 用戶端

當您將 NPS 設定為 RADIUS proxy 以將存取要求訊息轉送到其他 RADIUS 伺服器進行處理時，NPS 會作為 RADIUS 用戶端。 當您使用 NPS 做為 RADIUS proxy 時，需要下列一般設定步驟：

1. 網路存取伺服器，例如無線存取點和 VPN 伺服器，會使用 NPS proxy 的 IP 位址做為指定的 RADIUS 伺服器或驗證服務器來設定。 這可讓網路存取伺服器根據從存取用戶端接收的資訊建立存取要求訊息，以將訊息轉送到 NPS proxy。

2. NPS proxy 是藉由新增每個網路存取伺服器做為 RADIUS 用戶端來設定。 此設定步驟可讓 NPS proxy 從網路存取伺服器接收訊息，並在驗證期間與它們通訊。 此外，NPS proxy 上的連線要求原則會設定為指定要轉送至一或多個 RADIUS 伺服器的存取要求訊息。 這些原則也會使用遠端 RADIUS 伺服器群組進行設定，告訴 NPS 從網路存取伺服器接收訊息的位置。

3. Nps 或屬於 NPS proxy 上遠端 RADIUS 伺服器群組成員的其他 RADIUS 伺服器，會設定為接收來自 NPS proxy 的訊息。 這是藉由將 NPS proxy 設定為 RADIUS 用戶端來完成。

## <a name="radius-client-properties"></a>RADIUS 用戶端屬性

當您透過 NPS 主控台將 RADIUS 用戶端新增至 NPS 設定，或透過使用 NPS 或 Windows PowerShell 命令的 netsh 命令時，您會設定 NPS 從網路存取伺服器接收 RADIUS 存取要求訊息，或RADIUS proxy。

當您在 NPS 中設定 RADIUS 用戶端時，您可以指定下列屬性：

### <a name="client-name"></a>用戶端名稱

 RADIUS 用戶端的易記名稱，可讓您在使用 nps 嵌入式管理單元或 NPS 的 netsh 命令時更容易識別。

### <a name="ip-address"></a>IP 位址

網際網路通訊協定第4版 \(IPv4 @ no__t-1 位址或網域名稱系統 \(DNS @ no__t-3 RADIUS 用戶端的名稱。

### <a name="client-vendor"></a>用戶端-廠商

RADIUS 用戶端的廠商。 否則，您可以使用用戶端廠商的 RADIUS 標準值。

### <a name="shared-secret"></a>共用密碼

在 RADIUS 用戶端、RADIUS 伺服器和 RADIUS proxy 之間用來作為密碼的文字字串。 使用 [訊息驗證者] 屬性時，也會使用共用密碼做為加密 RADIUS 訊息的金鑰。 此字串必須在 RADIUS 用戶端和 NPS 嵌入式管理單元中設定。

### <a name="message-authenticator-attribute"></a>訊息驗證者屬性

在 RFC 2869 （「RADIUS 延伸」）中描述的訊息摘要 5 \(MD5 @ no__t-1 雜湊的整個 RADIUS 訊息。 如果 RADIUS 訊息驗證器屬性存在，就會進行驗證。 如果驗證失敗，則會捨棄 RADIUS 訊息。 如果用戶端設定需要 [訊息驗證者] 屬性，但它不存在，則會捨棄 RADIUS 訊息。 建議使用訊息驗證器屬性。

>[!NOTE]
>當您使用可延伸的驗證通訊協定 \(EAP @ no__t-1 驗證時，[訊息驗證者] 屬性是必要的，而且預設為啟用。 

如需 NPS 的詳細資訊，請參閱[網路原則伺服器（NPS）](nps-top.md)。


---
title: NPS Proxy 伺服器負載平衡
description: 您可以使用本主題來瞭解 Windows Server 2016 和 Windows 10 VPN 的功能。
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ee552c0202bff26cdcc58411b64cb342a4c7409e
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996462"
---
# <a name="nps-proxy-server-load-balancing"></a>NPS Proxy 伺服器負載平衡

適用於：Windows Server 2016

遠端驗證撥入使用者服務 (RADIUS) 用戶端，也就是網路存取伺服器，例如虛擬私人網路 (VPN) 伺服器和無線存取點、建立連線要求，並將它們傳送至 RADIUS 伺服器（例如 NPS）。 在某些情況下，NPS 可能會一次收到太多連線要求，因而導致效能降低或超載。 當 NPS 超載時，將更多 Nps 新增到您的網路並設定負載平衡是個不錯的主意。 當您將傳入的連線要求平均分散到多個 Nps，以避免一或多個 Nps 超載時，就稱為負載平衡。

負載平衡特別適用于：

- 使用可延伸驗證通訊協定的組織-傳輸層安全性 \( eap-tls \) 或受保護的可延伸驗證通訊協定 \( PEAP \) -tls 以進行驗證。 因為這些驗證方法會使用憑證進行伺服器驗證，以及使用者或用戶端電腦驗證，所以 RADIUS proxy 和伺服器上的負載會比使用密碼型驗證方法時還要多。
- 需要維持持續服務可用性的組織。
- 網際網路服務提供者 \( isp \) ，可外包其他組織的 VPN 存取權。 外包的 VPN 服務可能會產生大量的驗證流量。

有兩種方法可用來平衡傳送至 Nps 的連線要求負載：

- 設定網路存取伺服器，將連線要求傳送至多個 RADIUS 伺服器。 例如，如果您有20個無線存取點和兩部 RADIUS 伺服器，請將每個存取點設定為將連線要求傳送到這兩個 RADIUS 伺服器。 您可以藉由將存取伺服器設定為以指定的優先順序順序將連線要求傳送至多個 RADIUS 伺服器，以在每個網路存取伺服器上進行負載平衡和提供容錯移轉。 這種負載平衡方法通常適用于不會部署大量 RADIUS 用戶端的小型組織。
- 使用設定為 RADIUS proxy 的 NPS，將多個 Nps 或其他 RADIUS 伺服器之間的連線要求進行負載平衡。 例如，如果您有100的無線存取點、一個 NPS proxy 和三個 RADIUS 伺服器，您可以設定存取點以將所有流量傳送到 NPS proxy。 在 NPS proxy 上，設定負載平衡，讓 proxy 在三個 RADIUS 伺服器之間平均分配連線要求。 這種負載平衡方法最適合具有許多 RADIUS 用戶端和伺服器的中型和大型組織。

在許多情況下，負載平衡的最佳方法是將 RADIUS 用戶端設定為將連線要求傳送到兩個 NPS proxy 伺服器，然後設定 NPS proxy 在 RADIUS 伺服器之間進行負載平衡。 此方法提供 NPS proxy 和 RADIUS 伺服器的容錯移轉和負載平衡。

## <a name="radius-server-priority-and-weight"></a>RADIUS 伺服器優先順序和權數

在 NPS proxy 設定過程中，您可以建立遠端 RADIUS 伺服器群組，然後將 RADIUS 伺服器新增至每個群組。 若要設定負載平衡，每個遠端 RADIUS 伺服器群組都必須有一個以上的 RADIUS 伺服器。 新增群組成員，或建立 RADIUS 伺服器做為群組成員之後，您可以存取 [新增 RADIUS 伺服器] 對話方塊，在 [負載平衡] 索引標籤上設定下列專案：

- **優先順序**。 [優先順序] 指定 RADIUS 伺服器對 NPS proxy 伺服器的重要性順序。 優先順序層級必須指派為整數的值，例如1、2或3。 數位越低，NPS proxy 提供給 RADIUS 伺服器的優先順序越高。 例如，如果將最高優先順序指派給 RADIUS 伺服器，則 NPS proxy 會先將連線要求傳送到 RADIUS 伺服器。如果無法使用優先順序為1的伺服器，NPS 就會將連接要求傳送至優先順序2的 RADIUS 伺服器，依此類推。 您可以將相同的優先順序指派給多個 RADIUS 伺服器，然後使用權數設定來進行負載平衡。

- **權數**。 NPS 會使用此權數設定來判斷群組成員具有相同的優先順序層級時，要傳送到每個群組成員的連接要求數目。 權數設定必須指派介於1到100之間的值，而值則代表100% 的百分比。 例如，如果遠端 RADIUS 伺服器群組包含兩個優先順序等級為1且加權分級為50的成員，則 NPS proxy 會將50% 的連線要求轉送到每部 RADIUS 伺服器。

- [**高級設定**]。 這些容錯移轉設定會提供一種方式，讓 NPS 判斷遠端 RADIUS 伺服器是否無法使用。 如果 NPS 判斷 RADIUS 伺服器無法使用，它可以開始將連線要求傳送給其他群組成員。 使用這些設定，您可以設定 NPS proxy 在考慮要求捨棄之前，等待 RADIUS 伺服器回應的秒數;NPS proxy 將 RADIUS 伺服器識別為無法使用之前，已捨棄的要求數上限;在 NPS proxy 將 RADIUS 伺服器識別為無法使用之前，要求之間可以經過的秒數。

## <a name="configure-nps-proxy-load-balancing"></a>設定 NPS proxy 負載平衡

在設定負載平衡之前，請建立部署計畫，其中包含您需要多少個遠端 RADIUS 伺服器群組、哪些伺服器是每個特定群組的成員，以及每部伺服器的優先順序和權數設定。

>[!NOTE]
>接下來的步驟假設您已部署並設定 RADIUS 伺服器。

若要將 NPS 設定為 proxy 伺服器，並將來自 RADIUS 用戶端的連線要求轉送到遠端 RADIUS 伺服器，您必須採取下列動作：

1. 部署您的 RADIUS 用戶端 \( VPN 伺服器、撥號伺服器、終端機服務閘道伺服器、802.1 x 驗證交換器和 802.1 x 無線存取點， \) 並將其設定為將連線要求傳送到 NPS proxy 伺服器。

2. 在 NPS proxy 上，將網路存取伺服器設定為 RADIUS 用戶端。 如需詳細資訊，請參閱[設定 RADIUS 用戶端](./nps-radius-clients-configure.md)。

3. 在 NPS proxy 上，建立一或多個遠端 RADIUS 伺服器群組。 在此過程中，請將 RADIUS 伺服器新增至遠端 RADIUS 伺服器群組。 如需詳細資訊，請參閱[設定遠端 RADIUS 伺服器群組](./nps-crp-rrsg-configure.md)。

4. 在 NPS proxy 上，針對您新增至遠端 RADIUS 伺服器群組的每部 RADIUS 伺服器，按一下 [RADIUS 伺服器**負載平衡**] 索引標籤，然後設定 [**優先順序**]、[**權數**] 和 [**高級] 設定**。

5. 在 NPS proxy 上，設定連線要求原則，將驗證和帳戶處理要求轉送到遠端 RADIUS 伺服器群組。 您必須為每個遠端 RADIUS 伺服器群組建立一個連線要求原則。 如需詳細資訊，請參閱[設定連線要求原則](./nps-crp-configure.md)。
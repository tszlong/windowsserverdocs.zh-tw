---
title: NPS Proxy 伺服器負載平衡
description: 瞭解 NPS proxy 伺服器負載平衡，以及您可以用來平衡傳送至 NPSs 之連線要求負載的方法。
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 8788bf67b9688f9938b509eb9683c71ee2030208
ms.sourcegitcommit: decb6c8caf4851b13af271d926c650d010a6b9e9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/13/2021
ms.locfileid: "98177487"
---
# <a name="nps-proxy-server-load-balancing"></a>NPS Proxy 伺服器負載平衡

適用於：Windows Server 2016

遠端驗證撥入消費者服務 (RADIUS) 用戶端，也就是網路存取伺服器，例如虛擬私人網路 (VPN) 伺服器和無線存取點、建立連線要求，並將它們傳送至 RADIUS 伺服器，例如 NPS。 在某些情況下，NPS 可能會一次收到太多連線要求，進而導致效能降低或超載。 當 NPS 超載時，最好將更多 NPSs 新增至您的網路，並設定負載平衡。 當您將連入連線要求平均分配至多個 NPSs，以防止一或多個 NPSs 的多載時，即稱為「負載平衡」。

負載平衡特別適用于：

- 使用可延伸驗證的組織 Protocol-Transport 層安全性 \( eap-tls \) 或受保護的可延伸驗證通訊協定 \( PEAP \) -tls 以進行驗證。 由於這些驗證方法會使用憑證來進行伺服器驗證，而針對使用者或用戶端電腦驗證，因此 RADIUS proxy 和伺服器上的負載會比使用密碼型驗證方法時多。
- 需要維持持續服務可用性的組織。
- 網際網路服務提供者 \( isp \) ，可將 VPN 存取外包給其他組織。 外包的 VPN 服務可以產生大量的驗證流量。

有兩種方法可用來平衡傳送至 NPSs 的連接要求負載：

- 設定您的網路存取伺服器，以將連線要求傳送到多部 RADIUS 伺服器。 例如，如果您有20個無線存取點和兩部 RADIUS 伺服器，請將每個存取點設定為將連線要求傳送至這兩個 RADIUS 伺服器。 您可以藉由將存取伺服器設定為以指定的優先順序順序將連線要求傳送到多個 RADIUS 伺服器，藉以負載平衡並在每個網路存取伺服器提供容錯移轉。 這種負載平衡方法通常適用于不會部署大量 RADIUS 用戶端的小型組織。
- 使用設定為 RADIUS proxy 的 NPS，在多個 NPSs 或其他 RADIUS 伺服器之間進行連線要求的負載平衡。 例如，如果您有100的無線存取點、一個 NPS proxy 和三個 RADIUS 伺服器，您可以設定存取點，將所有流量傳送到 NPS proxy。 在 NPS proxy 上，設定負載平衡，讓 proxy 將連接要求平均分配給三個 RADIUS 伺服器。 這種負載平衡方法最適用于具有許多 RADIUS 用戶端和伺服器的中型和大型組織。

在許多情況下，負載平衡的最佳方法是設定 RADIUS 用戶端，將連線要求傳送至兩個 NPS proxy 伺服器，然後將 NPS proxy 設定為在 RADIUS 伺服器之間進行負載平衡。 這種方法可提供 NPS proxy 和 RADIUS 伺服器的容錯移轉和負載平衡。

## <a name="radius-server-priority-and-weight"></a>RADIUS 伺服器優先順序和權數

在 NPS proxy 設定過程中，您可以建立遠端 RADIUS 伺服器群組，然後將 RADIUS 伺服器新增至每個群組。 若要設定負載平衡，每個遠端 RADIUS 伺服器群組都必須有一個以上的 RADIUS 伺服器。 新增群組成員或建立 RADIUS 伺服器作為群組成員之後，您可以存取 [新增 RADIUS 伺服器] 對話方塊，在 [負載平衡] 索引標籤上設定下列專案：

- **優先權**。 Priority 會指定 RADIUS 伺服器對 NPS proxy 伺服器的重要性順序。 優先權層級必須指派為整數的值，例如1、2或3。 數位越低，NPS proxy 提供給 RADIUS 伺服器的優先順序就越高。 例如，如果 RADIUS 伺服器指派的優先順序最高，則 NPS proxy 會先將連線要求傳送給 RADIUS 伺服器;如果無法使用優先順序為1的伺服器，NPS 就會將連線要求傳送到優先順序為2的 RADIUS 伺服器，依此類推。 您可以將相同的優先順序指派給多個 RADIUS 伺服器，然後使用權數設定在兩者之間進行負載平衡。

- **權數**。 當群組成員具有相同的優先順序層級時，NPS 會使用此權數設定來決定要將多少個連接要求傳送至每個群組成員。 權數設定必須指派介於1到100之間的值，且值代表100% 的百分比。 例如，如果遠端 RADIUS 伺服器群組包含兩個成員，且優先順序等級為1且權數等級為50，則 NPS proxy 會將連線要求的50% 轉送到每個 RADIUS 伺服器。

- **Advanced 設定**。 這些容錯移轉設定提供了一種方法，讓 NPS 判斷遠端 RADIUS 伺服器是否無法使用。 如果 NPS 判定 RADIUS 伺服器無法使用，則可以開始將連線要求傳送給其他群組成員。 使用這些設定時，您可以設定 NPS proxy 在考慮要求捨棄之前，等候來自 RADIUS 伺服器回應的秒數;NPS proxy 將 RADIUS 伺服器識別為無法使用之前，已卸載的要求數目上限;以及 NPS proxy 將 RADIUS 伺服器識別為無法使用之前，在要求之間可以經過的秒數。

## <a name="configure-nps-proxy-load-balancing"></a>設定 NPS proxy 負載平衡

設定負載平衡之前，請建立部署計畫，其中包含您需要的遠端 RADIUS 伺服器群組數目、哪些伺服器是每個特定群組的成員，以及每部伺服器的優先順序和權數設定。

>[!NOTE]
>接下來的步驟假設您已部署並設定 RADIUS 伺服器。

若要設定 NPS 做為 proxy 伺服器，並將連線要求從 RADIUS 用戶端轉寄到遠端 RADIUS 伺服器，您必須採取下列動作：

1. 部署 RADIUS 用戶端 \( VPN 伺服器、撥號伺服器、終端機服務閘道伺服器、802.1 x 驗證交換器，以及 802.1 x 無線存取點， \) 並將其設定為將連線要求傳送到 NPS proxy 伺服器。

2. 在 NPS proxy 上，將網路存取伺服器設定為 RADIUS 用戶端。 如需詳細資訊，請參閱 [設定 RADIUS 用戶端](./nps-radius-clients-configure.md)。

3. 在 NPS proxy 上，建立一或多個遠端 RADIUS 伺服器群組。 在此過程中，請將 RADIUS 伺服器新增至遠端 RADIUS 伺服器群組。 如需詳細資訊，請參閱 [設定遠端 RADIUS 伺服器群組](./nps-crp-rrsg-configure.md)。

4. 在 NPS proxy 上，針對您新增到遠端 RADIUS 伺服器群組的每個 RADIUS 伺服器，按一下 [RADIUS 伺服器 **負載平衡** ] 索引標籤，然後設定 [ **優先順序**]、[ **權數**] 和 [ **Advanced] 設定**。

5. 在 NPS proxy 上，設定連線要求原則，將驗證和帳戶處理要求轉寄到遠端 RADIUS 伺服器群組。 您必須為每個遠端 RADIUS 伺服器群組建立一個連線要求原則。 如需詳細資訊，請參閱 [設定連接要求原則](./nps-crp-configure.md)。
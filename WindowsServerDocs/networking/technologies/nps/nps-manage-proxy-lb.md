---
title: NPS Proxy 伺服器負載平衡
description: 您可以使用本主題來了解 Windows Server 2016 和 Windows 10 VPN 功能和功能。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 04f466f7646fc5109af7379cab1240d8cd6829ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874399"
---
# <a name="nps-proxy-server-load-balancing"></a>NPS Proxy 伺服器負載平衡

適用於：Windows Server 2016

遠端驗證撥號使用者服務 (RADIUS) 用戶端，也就是網路存取伺服器，例如虛擬私人網路 (VPN) 伺服器以及無線存取點，會建立連線要求，並將它們傳送至 NPS 類的 RADIUS 伺服器。 在某些情況下，NPS 可能會收到太多的連線要求一次，導致效能降低或多載。 NPS 多載時，它是將多個 NPSs 新增至您的網路，並設定負載平衡是個好主意。 當您平均分散在多個 NPSs 間連入的連線要求，以防止一個或多個 NPSs 的多載時，它稱為負載平衡。

負載平衡時特別有用：

- 使用可延伸驗證通訊協定-傳輸層安全性的組織\(EAP-TLS\)或 受保護的可延伸驗證通訊協定\(PEAP\)-TLS 驗證。 因為這些驗證方法會使用憑證進行伺服器驗證，並針對 使用者 或 用戶端電腦驗證，RADIUS proxy 與伺服器上的負載是比使用密碼型驗證方法時更繁重。
- 要維持持續服務可用性所需的組織。
- 網際網路服務提供者\(Isp\) ，外包供其他組織的 VPN 存取。 委外的 VPN 服務可以產生大量的驗證流量。

有兩種方法可用來平衡負載的連線要求傳送至您 NPSs:

- 設定網路存取伺服器將連線要求傳送至多部 RADIUS 伺服器。 比方說，如果您有 20 的無線存取點和兩部 RADIUS 伺服器，設定每個存取點，將連接要求傳送至這兩個 RADIUS 伺服器。 您可以進行負載平衡，並藉由設定存取伺服器，將連接要求傳送至多部 RADIUS 伺服器，指定依優先順序提供在每個網路存取伺服器的容錯移轉。 此負載平衡方式通常最適合小型組織，不會部署大量的 RADIUS 用戶端。
- 您可以使用 NPS 設定為 RADIUS proxy 負載平衡多個 NPSs 或其他 RADIUS 伺服器之間的連線要求。 例如，如果您有 100 個的無線存取點、 一個 NPS proxy 和三部 RADIUS 伺服器，您可以設定的存取點，以將所有流量都傳送到 NPS proxy。 在 NPS proxy 上設定負載平衡，讓 proxy 將連線要求，三個 RADIUS 伺服器之間平均分散。 這種負載平衡方法最適合有許多的 RADIUS 用戶端和伺服器的中型和大型組織。

在許多情況下，負載平衡的最佳方法是設定來將連接要求傳送至兩個 NPS proxy 伺服器，然後設定 NPS proxy，以進行負載平衡，RADIUS 伺服器之間的 RADIUS 用戶端。 此方法提供容錯移轉和負載平衡 NPS proxy 與 RADIUS 伺服器。

## <a name="radius-server-priority-and-weight"></a>RADIUS 伺服器的優先順序與權重

在 NPS proxy 組態過程中，您可以建立遠端 RADIUS 伺服器群組，然後再新增至每個群組的 RADIUS 伺服器。 若要設定負載平衡，您必須使用每個遠端 RADIUS 伺服器群組的多部 RADIUS 伺服器。 新增群組成員時，或建立做為群組成員的 RADIUS 伺服器之後，您可以存取 [新增 RADIUS 伺服器] 對話方塊中，[負載平衡] 索引標籤上設定下列項目：

- **優先順序**。 優先順序指定 NPS proxy 伺服器之 RADIUS 伺服器的重要性的順序。 必須為整數，例如 1、 2 或 3 的值指派優先權層級。 較低數字，優先順序較高的 NPS proxy 可讓 RADIUS 伺服器。 例如，如果 RADIUS 伺服器會指派最高的優先順序為 1，NPS proxy 將連線要求傳送到 RADIUS 伺服器第一次;無法使用伺服器優先順序為 1 時，NPS 會接著將連線要求傳送至 RADIUS 伺服器，優先順序為 2，依此類推。 您可以將相同的優先權指派給多部 RADIUS 伺服器，，，然後使用 負載平衡它們之間的 權重設定。

- **權數**。 群組成員具有相同的優先順序層級時，NPS 會使用此權數設定，以判斷多少的連接要求傳送至每個群組成員。 權重設定必須指定介於 1 和 100 之間的值，而值則代表 100%的百分比表示。 比方說，如果遠端 RADIUS 伺服器群組包含兩個成員都有優先順序層級 1 和 50 的權數評等，NPS proxy 會轉送連線要求每個 RADIUS 伺服器以 50%。

- **進階設定**。 這些容錯移轉設定可判斷是否無法使用遠端 RADIUS 伺服器的 nps。 如果 NPS 會判斷為 RADIUS 伺服器，就無法使用，它可以開始將連線要求傳送至其他群組成員。 使用這些設定中，您可以設定 NPS proxy 等候從 RADIUS 伺服器的回應之前將它視為卸除; 要求的秒數NPS proxy 之前，已卸除要求的數目上限會識別為無法使用; 的 RADIUS 伺服器和 NPS proxy 之前，要求之間所經過的秒數會識別為無法使用的 RADIUS 伺服器。

## <a name="configure-nps-proxy-load-balancing"></a>設定 NPS proxy 負載平衡

設定負載平衡之前, 建立的部署計劃包含幾個遠端 RADIUS 伺服器群組在需要時，哪些伺服器屬於每個特定的群組及每一部伺服器的優先順序和權數設定。

>[!NOTE]
>請依照下列步驟假設您有已部署並設定 RADIUS 伺服器。

若要設定 NPS 做為 proxy 伺服器和遠端 RADIUS 伺服器從 RADIUS 用戶端轉寄連線要求，您必須採取下列動作：

1. 部署您的 RADIUS 用戶端\(VPN 伺服器、 撥號伺服器、 終端機服務閘道伺服器、 802.1 X 驗證交換器和 802.1x 無線存取點\)並將其傳送連線要求給 NPS proxy 設定伺服器。

2. 在 NPS proxy 上設定網路存取伺服器做為 RADIUS 用戶端。 如需詳細資訊，請參閱 <<c0> [ 設定 RADIUS 用戶端](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure)。

3. 在 NPS proxy，建立一或多個遠端的 RADIUS 伺服器群組。 在此過程中，加入遠端 RADIUS 伺服器群組中的 RADIUS 伺服器。 如需詳細資訊，請參閱 <<c0> [ 設定遠端 RADIUS 伺服器群組](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure)。

4. 在您新增到遠端 RADIUS 伺服器群組，每個 RADIUS 伺服器的 NPS proxy，按一下 RADIUS 伺服器**負載平衡**索引標籤，然後再設定**優先順序**，**權數**與**進階設定**。

5. 在 NPS proxy 設定連線要求原則，以將驗證和帳戶處理要求轉送到遠端 RADIUS 伺服器群組。 您必須建立一個連線要求原則，每個遠端 RADIUS 伺服器群組。 如需詳細資訊，請參閱 <<c0> [ 設定連線要求原則](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure)。



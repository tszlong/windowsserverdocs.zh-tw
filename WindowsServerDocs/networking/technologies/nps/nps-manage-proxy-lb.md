---
title: NPS Proxy 伺服器負載平衡
description: 若要深入了解 Windows Server 2016 和 Windows 10 VPN 功能，您可以使用此主題。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9f00eb6575284a69358c58b36d08672cdd69dc18
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="nps-proxy-server-load-balancing"></a>NPS Proxy 伺服器負載平衡

適用於：Windows Server 2016

遠端驗證 Dial 使用者服務 (RADIUS) 戶端（例如私人網路 virtual (VPN) 伺服器以及 wireless 存取點的網路存取伺服器），建立連接要求，例如 NPS RADIUS 伺服器將它們傳送給。 有時候，NPS 伺服器可能會收到太多連接要求一次，會導致效能降低」或「多載。 當多載 NPS 伺服器是新增到您的網路，以及設定的更多 NPS 伺服器負載平衡最好的做法。 當您平均散發多個 NPS 伺服器之間傳入的連接要求，以避免的一或多個 NPS 伺服器載時，稱為負載平衡。

負載平衡是適合用來：

- 使用最具擴充性驗證通訊協定-Tls \(EAP-TLS\) 或受延伸驗證通訊協定的組織 \ (PEAP\)-TLS 進行驗證。 這些驗證方法使用憑證的伺服器的驗證，以及使用者或 client 電腦驗證，因為載入 RADIUS proxy 伺服器上的為重密碼架構的驗證方法使用時於。
- 必須以維持連續服務的可用性的組織。
- 網際網路服務提供者 \(ISPs\) 的外包 VPN 存取其他組織。 大量的驗證資料傳輸產生外部的 VPN 服務。

有兩種方法，您可以使用平衡連接要求傳送給您 NPS 伺服器的負載：

- 設定多個 RADIUS 伺服器傳送請求連接您網路的存取伺服器。 例如，如果您 20 wireless 存取點兩個 RADIUS 伺服器、設定將連接要求傳送給這兩個 RADIUS 伺服器每個存取點。 您可以負載平衡，並且設定將連接要求傳送給指定的優先順序訂單中的多個 RADIUS 伺服器存取伺服器提供容錯在每個網路的存取伺服器移轉。 此負載平衡最適合方法通常是小型不部署的 RADIUS 用大量的組織。
- 使用 NPS RADIUS proxy 設定負載平衡連接要求之間多個 NPS 伺服器或其他 RADIUS 伺服器。 例如，如果您有 100 wireless 存取點，一個 NPS proxy，並三個 RADIUS 伺服器，您可以設定的存取點，將所有資料傳輸 NPS proxy 給。 在 [NPS proxy 設定負載平衡使 proxy 平均分配之間的三個 RADIUS 伺服器連接要求。 有許多 RADIUS 戶端與伺服器中型與大型的組織適合負載平衡此方法。

很多時候，負載平衡的最佳方式是將 RADIUS 伺服器連接要求傳送給兩個 NPS proxy 伺服器，並設定進行負載平衡之間 RADIUS 伺服器 NPS proxy 設定。 這種方式提供容錯移轉與負載平衡，proxy NPS RADIUS 伺服器。

## <a name="radius-server-priority-and-weight"></a>RADIUS 伺服器優先順序和減重

在 NPS proxy 設定過程中，您可以建立遠端 RADIUS 伺服器群組，然後再新增每個群組的 [RADIUS 伺服器。 若要設定負載平衡，您必須遠端 RADIUS 伺服器群組每一部以上的 RADIUS 伺服器。 加入群組，同時，或建立 RADIUS 伺服器群組成員後，您可以存取新增 RADIUS 伺服器對話方塊負載平衡] 索引標籤上，設定下列項目：

- **優先順序**。 優先順序指定的 proxy 伺服器 NPS RADIUS 伺服器的重要性的順序。 必須值，可為整數，例如 1、2 或 3 指派優先順序層級。 越低數字，較高優先順序 NPS proxy RADIUS 伺服器提供。 例如，如果 RADIUS 伺服器指定為 1 最高優先順序，NPS proxy 傳送連接要求 RADIUS 伺服器第一次。如果優先順序 1 伺服器無法使用，NPS 會傳送連接請求給 RADIUS 伺服器優先順序 2 等等。 您可以相同的優先順序為多個 RADIUS 伺服器，並進行負載平衡之間使用減重設定。

- **減重**。 NPS 使用這個減重設定，以判斷多少連接要求傳送給為每個群組成員時群組成員有相同的優先順序層級。 介於 1 與 100，必須指派重量設定和百分比 100%代表值。 例如，如果遠端 RADIUS 伺服器群組包含優先順序層級 1 和的 50 減重評分兩者有兩個成員，NPS proxy 轉送 50%的每個 RADIUS 伺服器連接請求。

- **進階設定**。 這些容錯移轉」設定提供的 NPS 判斷是否遠端 RADIUS 伺服器無法使用的方式。 如果 NPS RADIUS 伺服器不是可用，就可以開始連接要求傳送給其他群組成員。 利用這些設定，您可以設定 NPS proxy 等待 RADIUS 伺服器回應之前它認為卸除; 要求秒的數卸除要求 NPS proxy 之前的上限辨識 RADIUS 伺服器無法使用。並秒之間 NPS proxy 之前，要求可經過數找出 RADIUS 伺服器無法使用。

## <a name="configure-nps-proxy-load-balancing"></a>設定 NPS proxy 負載平衡

設定之前負載平衡，建立部署計畫，包括如何的許多遠端 RADIUS 伺服器群組您需要時，所伺服器是每個特殊群組，與每個伺服器的優先順序和減重設定的成員。

>[!NOTE]
>請依照下列步驟進行假設您擁有已經部署，並設定 RADIUS 伺服器。

若要設定 NPS 做為 proxy 伺服器並向前連接要求 RADIUS 從遠端 RADIUS 伺服器，您必須執行下列動作：

1. 部署 RADIUS 戶端 \（VPN 伺服器、撥號伺服器、車票服務閘道伺服器、802.1 X 驗證參數，以及 802.1 X 無線存取 points\）並將其連接要求傳送給您 NPS proxy 伺服器設定。

2. NPS proxy，將設定為 RADIUS 戶端的網路存取伺服器。 如需詳細資訊，請查看[設定 RADIUS 戶端](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure)。

3. NPS proxy，建立一個或多個遠端的 RADIUS 伺服器群組。 在此過程中，將 RADIUS 伺服器加入遠端 RADIUS 伺服器群組。 如需詳細資訊，請查看[設定遠端 RADIUS 伺服器群組](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure)。

4. NPS proxy，為您新增到遠端的 RADIUS 伺服器群組，每個 RADIUS 伺服器上按一下 RADIUS 伺服器**負載平衡**索引標籤，然後再設定**優先順序**、**減重**，和**進階設定**。

5. NPS proxy，來設定連接要求原則遠端 RADIUS 伺服器群組轉送給驗證及計量要求。 您必須建立遠端 RADIUS 伺服器群組每一個連接要求原則。 如需詳細資訊，請查看[設定連接要求原則](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure)。



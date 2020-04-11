---
title: 遠端桌面 - 允許從網路外部存取您的電腦
description: 深入了解您從電腦網路外部遠端存取電腦的選項
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: haley-rowland
manager: dongill
ms.author: elizapo
ms.date: 04/04/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9cc1b7568006ef9e32132d772702212c5fd78ec4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857421"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>遠端桌面 - 允許從電腦網路外部存取您的電腦

>適用於：Windows 10、Windows Server 2016

當您使用遠端桌面用戶端連線至您的電腦時，您會建立對等連線。 這表示您需要直接存取電腦 (有時稱為「主機」)。 如果您需要從電腦執行所在的網路外部連線至電腦，則必須啟用該存取權。 您有幾個選項：使用連接埠轉送或設定 VPN。

## <a name="enable-port-forwarding-on-your-router"></a>在路由器上啟用連接埠轉送

連接埠轉送會直接將您的路由器 IP 位址 (您的公用 IP) 上的連接埠對應至您要存取之電腦的連接埠和 IP 位址。 

啟用連接埠轉送的具體步驟取決於您所使用的路由器，因此您必須在線上搜尋您路由器的指示。 如需相關步驟的一般討論，請參閱 [Wiki：如何在路由器上設定連接埠轉送](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router) (英文)。

對應連接埠之前，您將需要下列項目：

- 電腦的內部 IP 位址：請查看 [設定 > 網路和網際網路 > 狀態 > 檢視您的網路內容]  。 尋找處於「可操作」狀態的網路設定，然後取得 **IPv4 位址**。

   ![可操作的網路設定](../media/rdclient-operational-network.png)

- 您的公用 IP 位址 (路由器的 IP)。 有許多方法可以找到這項資訊 - 您可以在 Bing 或 Google 中搜尋「我的 IP」，或檢視 [Wi-Fi 網路內容](https://binged.it/2Gwob34) (適用於 Windows 10)。
- 要對應的連接埠號碼。 在大部分情況下，此連接埠為 3389 - 這是遠端桌面連線所使用的預設連接埠。
- 系統管理員對您路由器的存取權。  

   >[!WARNING]
   > 您將對網際網路開放您的電腦 - 請確實為您的電腦設定強式密碼。

對應連接埠後，您將可連線至路由器的公用 IP 位址 (前述的第二項重點)，從區域網路外部連線至您的主機電腦。

路由器的 IP 位址可能會變更 - 網際網路服務提供者 (ISP) 可能會隨時為您指派新的 IP。 若要避免發生此問題，請考慮使用動態 DNS - 這樣可讓您使用好記的網域名稱 (而不是 IP 位址) 連線至電腦。 一旦 IP 位址有所變更，路由器即會以新的 IP 位址自動更新 DDNS 服務。

對於大部分的路由器，您都可以定義哪個來源 IP 或來源網路可使用連接埠對應。 因此，如果您知道您只會從工作場所連線，您就可以為工作場所網路新增 IP 位址 - 這樣可讓您避免對整個公用網際網路開放連接埠。 如果您用來連線的主機使用動態 IP 位址，請設定來源限制，以允許該 ISP 的整個範圍所進行的存取。

您也可以考慮在您的電腦上設定[靜態 IP 位址](/windows-hardware/customize/mobile/mcsf/enable-static-ip)，而使內部 IP 位址不會變更。 如果您這麼做，路由器的連接埠轉送將一律指向正確的 IP 位址。


## <a name="use-a-vpn"></a>使用 VPN

如果您使用虛擬私人網路 (VPN) 連線至區域網路，則無須對公用網際網路開放您的電腦。 在此情況下，當您連線至 VPN 時，RD 用戶端的運作方式會像是它位於相同網路中，且能夠存取您的電腦。 目前有許多 VPN 服務可供使用 - 您可以尋找並使用最適合自己的服務。
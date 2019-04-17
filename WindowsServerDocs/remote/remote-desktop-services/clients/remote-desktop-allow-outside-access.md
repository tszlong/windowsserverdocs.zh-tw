---
title: 遠端桌面-允許存取您從網路外部的電腦
description: 了解可用於從遠端存取您從電腦的網路以外的電腦的選項
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: haley-rowland
manager: dongill
ms.author: elizapo
ms.date: 04/04/2018
ms.localizationpriority: medium
ms.openlocfilehash: d77c362d9d06b70ad0747002ed8853a39e05b7ff
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2018
ms.locfileid: "1708656"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>遠端桌面-允許存取您從您電腦的網路以外的電腦

>適用於： Windows 10、 Windows Server 2016

當您使用遠端桌面用戶端連線至您的電腦時，您建立的對等連線。 這表示您需要的直接存取權 （有時稱為 「 主機 」） 的電腦。 如果您需要連線至您從您的電腦執行的網路以外的電腦，您需要啟用的存取。 您有幾個選項： 使用連接埠轉寄或已設定 VPN。

## <a name="enable-port-forwarding-on-your-router"></a>在您的路由器上啟用連接埠轉寄

連接埠轉接只是將在您的路由器的 IP 位址 (您公用 IP) 的連接埠對應至連接埠與您要存取電腦的 IP 位址。 

啟用連接埠轉接的特定步驟取決於您使用、 路由器，讓您將需要線上搜尋您的路由器指示。 如需步驟的一般資訊，查看[wikiHow 來設定向上連接埠轉接路由器上](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router)。

對應的連接埠之前將需要下列各項：

- 電腦內部 IP 位址： 查看**設定 > 網路與網際網路 > 狀態 > 檢視網路屬性**。 尋找"操作 」 狀態的網路組態，並再取得**IPv4 位址**。

   ![操作網路設定](../media/rdclient-operational-network.png)

- 您公用 IP 位址 (路由器的 IP)。 有許多方法來尋找此-您可以搜尋 （Bing 或 Google） 中的 「 我的 IP"或檢視[Wi-fi 網路內容](https://binged.it/2Gwob34)（適用於 Windows 10)。
- 對應的連接埠號碼。 在大多數情況下這是 3389-這是遠端桌面連線所使用的預設連接埠。
- 您的路由器管理員權限。  

   >[!WARNING]
   > 您正在開啟您最多個網際網路的電腦-請確定您已經設定您的電腦的強式密碼。

對應的連接埠之後，您將能夠連線到主機電腦從網路外部的本機機連線到您的路由器 （第二個項目符號上述） 的公用 IP 位址。

路由器的 IP 位址可以變更-網際網路服務提供者 (ISP) 可以指派您新增 IP 任何時候。 若要避免執行這個問題，請考慮使用動態 DNS-這可讓您連線至 PC 使用容易記住的網域名稱，而不是 IP 位址。 您的路由器自動更新 DDNS 服務與新的 IP 位址，它應該變更。

您可以使用大部分的路由器定義哪個來源 IP 或來源網路可以使用的連接埠對應。 因此，如果您知道您只些工時從連線，您可以為您的公司網路-新增的 IP 位址，可讓您避免開啟整個公用網際網路的連接埠。 如果您使用連線的主機使用動態 IP 位址，設定為允許存取該特定的 ISP 整個範圍中的來源限制。

您也可能會考慮設定[靜態 IP 位址](/windows-hardware/customize/mobile/mcsf/enable-static-ip)在電腦上讓內部 IP 位址不會變更。 如果您這樣做，則為路由器連接埠轉接一定會指向正確的 IP 位址。


## <a name="use-a-vpn"></a>使用 VPN

如果您使用虛擬私人網路 (VPN) 連線至區域網路，您不需要開啟您公用網際網路的電腦。 而當您連線至 VPN，RD 用戶端作用就像它是位於相同網路的一部分且能夠存取您的電腦。 有可用的數目 VPN services-您可以找到並使用準最適合您。
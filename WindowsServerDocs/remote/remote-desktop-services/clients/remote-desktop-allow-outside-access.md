---
title: 遠端桌面-允許您從網路外部的電腦存取
description: 深入了解您的選項，從遠端存取您從電腦的網路外部的電腦
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
ms.openlocfilehash: 9e90a2faa14b65bc766c7d7ec47d5e815658c06e
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805066"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>遠端桌面-允許您從您的電腦網路外部的電腦存取

>適用於：Windows 10,  Windows Server 2016

當您使用遠端桌面用戶端連線至您的電腦時，您建立的對等連線。 這表示您需要直接存取 （有時稱為 「 主機 」） 的電腦。 如果您需要連線到您的電腦無法執行您的電腦的網路之外，您必須啟用該存取權。 您有幾個選項： 使用連接埠轉送或將 vpn 設定。

## <a name="enable-port-forwarding-on-your-router"></a>啟用您的路由器上的連接埠轉送

連接埠轉送只會將路由器的 IP 位址 (您公用 IP) 上的連接埠對應至您想要存取電腦的 IP 位址與連接埠。 

因此您必須在線上搜尋您的路由器的指示，啟用連接埠轉送的特定步驟會取決於您使用，路由器。 如需步驟的一般討論，請參閱[若要設定設定的連接埠轉送的路由器上的 wikiHow](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router)。

對應連接埠之前，您將需要下列項目：

- 電腦的內部 IP 位址：查看**設定 > 網路和網際網路 > 狀態 > 檢視您的網路屬性**。 尋找 「 操作 」 狀態的網路組態，然後取得**IPv4 位址**。

   ![作業的網路組態](../media/rdclient-operational-network.png)

- 公用 IP 位址 (路由器的 IP)。 有許多方法可以找到此-可以在 [我的 IP] （在 Bing 或 Google） 搜尋或檢視[Wi-fi 網路內容](https://binged.it/2Gwob34)（適用於 Windows 10)。
- 要對應的連接埠號碼。 在大部分情況下，這是 3389-這是使用遠端桌面連線的預設連接埠。
- 系統管理員存取您的路由器。  

   >[!WARNING]
   > 您開啟您最多網際網路的電腦-請確定您已為您的電腦設定強式密碼。

對應的連接埠之後，您可以連接到您主機電腦從網路外部的本機連接到您的路由器 （第二個上述項目） 的公用 IP 位址。

路由器的 IP 位址可能會變更-您的網際網路服務提供者 (ISP) 可以指派給您的新 IP 在任何時間。 若要避免發生此問題，請考慮使用動態 DNS-這可讓您連接至 PC 使用好記的網域名稱，而不是 IP 位址。 您的路由器會自動更新 DDNS 服務新的 IP 位址，它應該變更。

您可以使用大部分的路由器，定義哪些來源 IP 」 或 「 來源網路，則可以使用連接埠對應。 因此，如果您知道您只會從公司連線，您就可以為您的工作網路-新增的 IP 位址，可讓您避免開啟整個公用網際網路的連接埠。 如果您用來連接的主機會使用動態 IP 位址，請將來源限制以允許從該特定的 ISP 的整個範圍的存取設定。

您也可以考慮設定[靜態 IP 位址](/windows-hardware/customize/mobile/mcsf/enable-static-ip)上您的電腦，因此不會變更的內部 IP 位址。 如果您這樣做，則路由器的連接埠轉送永遠會指向正確的 IP 位址。


## <a name="use-a-vpn"></a>使用 VPN

如果您使用虛擬私人網路 (VPN) 連線到您的區域網路，您不必開啟您在公用網際網路的電腦。 相反地，當您連接至 VPN，您的 RD 用戶端行為就像是它屬於相同的網路，且能夠存取您的電腦。 有許多 VPN 服務-您可以尋找並使用較適合您。
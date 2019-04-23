---
title: 設定多重主目錄電腦上的 NPS
description: 本主題提供使用網路原則伺服器執行 Windows Server 2016 中的多個網路介面卡設定伺服器的指示。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 55eccf3afc649e84c5b6f5ce7932ed97617ddca9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856799"
---
# <a name="configure-nps-on-a-multihomed-computer"></a>設定多重主目錄電腦上的 NPS

>適用於：Windows Server （半年通道），Windows Server 2016

若要使用多張網路介面卡設定 NPS，您可以使用本主題。

當您在執行網路原則伺服器 (NPS) 的伺服器中使用多張網路介面卡時，您可以設定下列各項：

- 執行，以及執行未傳送和接收遠端驗證撥入使用者服務的網路介面卡\(RADIUS\)流量。
- 針對每個網路介面卡，NPS 監視網際網路通訊協定第 4 版上的 RADIUS 流量\(IPv4\)，IPv6 或 IPv4 和 IPv6 兩者。
- 透過 RADIUS，流量會傳送和接收的每個通訊協定的 UDP 連接埠號碼\(IPv4 或 IPv6\)，每個網路介面卡為基礎。

根據預設，NPS 會接聽連接埠 1812年、 1813年、 1645年和 1646年上的 RADIUS 流量的 IPv6 和 IPv4 所有已安裝的網路介面卡。 因為 NPS 自動使用所有網路介面卡用於 RADIUS 流量，您只需要指定網路介面卡，想要 NPS 用於 RADIUS 流量，當您想要避免 NPS 將特定網路介面卡。

>[!NOTE]
>如果您解除安裝 IPv4 或 IPv6 網路介面卡上，NPS 不會監視已解除安裝的通訊協定的 RADIUS 流量。

上已安裝的多個網路介面卡的 NPS，您可能想要設定 NPS 來傳送和接收 RADIUS 流量，只在您指定的介面卡上。

比方說，不包含 RADIUS 用戶端，而第二個網路介面卡提供 NPS 的網路路徑為其設定的 RADIUS 用戶端的網路區段可能會導致安裝在 NPS 中的一個網路介面卡。 在此案例中，是引導 NPS 對所有的 RADIUS 流量使用第二個網路介面卡。

另舉一例，如果您的 NPS 具有三個網路介面卡安裝，但您只想要 NPS 用於 RADIUS 流量使用其中二張您可以設定兩個的介面卡的通訊埠資訊。 藉由排除第三個配接器的連接埠組態，可以避免 NPS 用於 RADIUS 流量使用配接器。

## <a name="using-a-network-adapter"></a>使用的網路介面卡

若要設定 NPS 監聽並傳送 RADIUS 流量的網路介面卡，請在 網路原則伺服器的內容對話方塊中，在 NPS 主控台中使用下列語法：

- IPv4 流量語法：Ipaddress: Udpport，其中 IPAddress 是設定您要對其傳送 RADIUS 流量的網路介面卡的 IPv4 位址，而 UDPport 是您想要用於 RADIUS 驗證或帳戶處理流量的 RADIUS 連接埠號碼。
- IPv6 流量語法: [IPv6Address]:UDPport，其中所需的括號括住 IPv6Address IPv6Address 設定您想要傳送 RADIUS 流量的網路介面卡的 IPv6 位址，是 UDPport 是您想要用於 RADIUS 驗證的 RADIUS 連接埠號碼或帳戶處理流量。

下列字元可用來當做分隔符號的設定 IP 位址與 UDP 連接埠資訊：

- 位址/連接埠分隔字元： 冒號 （:）
- 連接埠分隔字元： 逗號 （，）
- 介面分隔字元： 分號 （;）

## <a name="configuring-network-access-servers"></a>設定網路存取伺服器

請確定您的網路存取伺服器已與您在您 NPSs 設定的相同 RADIUS UDP 連接埠號碼。 RADIUS 標準 UDP 連接埠在 Rfc 2865 與 2866年中定義是 1812 用於驗證而 1813 用於帳戶處理;不過，有些存取伺服器的 UDP 連接埠 1645年用於驗證要求和 UDP 連接埠 1646年用於帳戶處理要求的預設設定。

>[!IMPORTANT]
>如果您不使用 RADIUS 預設連接埠號碼，您必須設定防火牆以允許新連接埠上的 RADIUS 流量的本機電腦上的例外狀況。 如需詳細資訊，請參閱 <<c0> [ 設定用於 RADIUS 流量的防火牆](nps-firewalls-configure.md)。

## <a name="configure-the-multihomed-nps"></a>設定多重主目錄 NPS

若要設定您的多重主目錄 NPS，您可以使用下列程序。

若要完成此程序，至少需要 **Domain Admins** 的成員資格或同等權限。

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>若要指定網路介面卡和 NPS 用於 RADIUS 流量的 UDP 連接埠

1. 在 [伺服器管理員] 中，按一下**工具**，然後按一下**網路原則伺服器**開啟 NPS 主控台。

2. 以滑鼠右鍵按一下**網路原則伺服器**，然後按一下**屬性**。

3. 按一下 **連接埠**索引標籤，並在前面加上您想要使用現有的連接埠號碼的 RADIUS 流量的網路介面卡的 IP 位址。 比方說，如果您想要的 IP 位址 192.168.1.2 及 RADIUS 連接埠 1812年與 1645年用於驗證要求時，變更連接埠設定，從**1812，1645**要**192.168.1.2:1812,1645**。 如果 RADIUS 驗證與 RADIUS 帳戶處理 UDP 連接埠的預設值不同，請變更通訊埠設定據以。

4. 若要使用多個通訊埠設定進行驗證或帳戶處理要求，請以逗號分隔的連接埠號碼。

如需 NPS UDP 連接埠的詳細資訊，請參閱[設定 NPS UDP 連接埠資訊](nps-udp-ports-configure.md)


如需 NPS 的詳細資訊，請參閱[網路原則伺服器](nps-top.md)


---
title: 設定多重主目錄電腦上的 NPS
description: 本主題提供的指示說明如何設定具有多個網路介面卡（在 Windows Server 2016 中執行網路原則伺服器）的伺服器。
manager: brianlic
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1292accf4d19afa7f6a050281b7af373b3c867c3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952083"
---
# <a name="configure-nps-on-a-multihomed-computer"></a>設定多重主目錄電腦上的 NPS

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來設定具有多個網路介面卡的 NPS。

在執行網路原則伺服器 (NPS) 的伺服器中使用多張網路介面卡時，可以設定下列各項：

- 執行和不會傳送和接收遠端驗證撥入使用者服務 RADIUS 流量的網路 \( 配接器 \) 。
- 以每個網路介面卡為基礎，NPS 會監視網際網路通訊協定第4版 \( ipv4 \) 、Ipv6 或 Ipv4 和 IPV6 的 RADIUS 流量。
- UDP 埠號碼，會根據每個通訊協定的 \( IPv4 或 IPv6 \) 、每個網路介面卡來傳送和接收 RADIUS 流量。

根據預設值，NPS 會為所有已安裝的網路介面卡的 IPv6 與 IPv4，接聽連接埠 1812、1813、1645 和 1646 上的 RADIUS 流量。 由於 NPS 會自動針對 RADIUS 流量使用所有網路介面卡，因此當您想要防止 NPS 使用特定的網路介面卡時，您只需要指定您想要 NPS 用於 RADIUS 流量的網路介面卡。

>[!NOTE]
>如果您解除安裝網路介面卡上的 IPv4 或 IPv6，NPS 不會監視已解除安裝的通訊協定的 RADIUS 流量。

在已安裝多個網路介面卡的 NPS 上，您可能會想要設定 NPS 只在您指定的介面卡上傳送和接收 RADIUS 流量。

例如，安裝在 NPS 中的一張網路介面卡可能會導致不包含 RADIUS 用戶端的網路區段，而第二張網路介面卡則會為 NPS 提供其設定之 RADIUS 用戶端的網路路徑。 在此情況下，重要的是引導 NPS 對所有 RADIUS 流量使用第二張網路介面卡。

在另一個範例中，如果您的 NPS 已安裝三張網路介面卡，但您只想要 NPS 使用兩個介面卡來進行 RADIUS 流量，您只能設定兩個介面卡的埠資訊。 藉由排除第三張網路介面卡的連接埠設定，可以避免 NPS 將該介面卡用於 RADIUS 流量。

## <a name="using-a-network-adapter"></a>使用網路介面卡

若要設定 NPS 在網路介面卡上接聽和傳送 RADIUS 流量，請在 NPS 主控台的 [網路原則伺服器] 的 [內容] 對話方塊中使用下列語法：

- IPv4 流量語法： IPAddress： UDPport，其中 IPAddress 是在您要傳送 RADIUS 流量的網路介面卡上設定的 IPv4 位址，而 UDPport 是您要用於 RADIUS 驗證或帳戶處理流量的 RADIUS 埠號碼。
- IPv6 流量語法： [IPv6Address]： UDPport，其中需要 IPv6Address 的括弧，IPv6Address 是在您要用來傳送 RADIUS 流量的網路介面卡上設定的 IPv6 位址，而 UDPport 是您要用於 RADIUS 驗證或帳戶處理流量的 RADIUS 埠號碼。

下列字元可在設定 IP 位址與 UDP 連接埠資訊時當作分隔字元：

- 位址/埠分隔符號：冒號 (： ) 
- 埠分隔符號：逗號 (，) 
- 介面分隔符號：分號 (; ) 

## <a name="configuring-network-access-servers"></a>設定網路存取伺服器

請確定您的網路存取伺服器已設定為使用您在 Nps 上設定的相同 RADIUS UDP 埠號碼。 RFC 2865 與 2866 中定義的 RADIUS 標準 UDP 連接埠是 1812 用於驗證而 1813 用於帳戶處理；不過，有些存取伺服器的設定預設將 UDP 連接埠 1645 用於驗證要求，而將 UDP 連接埠 1646 用於帳戶處理要求。

>[!IMPORTANT]
>如果您不是使用 RADIUS 預設連接埠號碼，必須為本機電腦的防火牆設定例外，允許新連接埠上的 RADIUS 流量。 如需詳細資訊，請參閱[設定 RADIUS 流量的防火牆](nps-firewalls-configure.md)。

## <a name="configure-the-multihomed-nps"></a>設定多重主目錄 NPS

您可以使用下列程式來設定多重主目錄 NPS。

至少要有 **Domain Admins** 的成員資格或是對等成員資格，才能完成此程序。

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>指定 NPS 用於 RADIUS 流量的網路介面卡與 UDP 連接埠

1. 在 [伺服器管理員] 中，按一下 [**工具**]，然後按一下 [**網路原則伺服器**] 以開啟 NPS 主控台。

2. 以滑鼠右鍵按一下 [**網路原則伺服器**]，**然後按一下 [** 內容]。

3. 按一下 [**埠**] 索引標籤，然後在您要用於 RADIUS 流量的網路介面卡前面加上現有埠號碼的 IP 位址。 例如，如果您想要針對驗證要求使用 IP 位址192.168.1.2 和 RADIUS 埠1812和1645，請將埠設定從**1812、1645**變更為**192.168.1.2：1812、1645**。 如果 RADIUS 驗證與 RADIUS 帳戶處理 UDP 連接埠與預設值不同，請分別變更連接埠設定。

4. 驗證或帳戶處理要求若要使用多個連接埠設定，請以逗號分隔連接埠號碼。

如需 NPS UDP 埠的詳細資訊，請參閱[設定 NPS Udp 埠資訊](nps-udp-ports-configure.md)


如需 NPS 的詳細資訊，請參閱[網路原則伺服器](nps-top.md)


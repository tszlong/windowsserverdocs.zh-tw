---
title: 在 [多重位址的電腦上設定 NPS
description: 本主題提供的網路原則伺服器執行 Windows Server 2016 中的多個網路介面卡設定伺服器上的指示。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f80e83a4d79036729b6b442e6362d52fbda12edd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-nps-on-a-multihomed-computer"></a>在 [多重位址的電腦上設定 NPS

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題有多個網路介面卡設定 NPS 伺服器。

當您在執行的網路原則 Server (NPS) 伺服器使用多個網路介面卡時，您可以設定下列各項：

- 執行，並執行未傳送和接收撥號使用者服務遠端驗證 \(RADIUS\) 流量網路介面卡。
- 在每次網路介面卡，是否 NPS 監視 RADIUS 流量網際網路通訊協定第 4 版 \(IPv4\)、IPv6，或 IPv4 和 IPv6 上。
- 透過哪些 RADIUS 交通傳送和接收上每一位通訊協定 UDP 連接埠號碼 \（IPv4 或 IPv6\）-網路介面卡的方式。

根據預設，NPS 接聽 RADIUS 流量連接埠 1812 年，1813 年、1645 年 1646 IPv6 與 IPv4 所有已安裝的網路介面卡。 因為 NPS RADIUS 流量自動使用所有網路介面卡，您只需要指定您希望 NPS RADIUS 使用的網路介面卡流量當您想要使用的特定網路介面卡，防止 NPS。

>[!NOTE]
>如果您解除安裝 IPv4 或 IPv6 網路介面卡、NPS 不會監視 RADIUS 傳輸通訊協定解除安裝。

NPS 在伺服器上已安裝多個網路介面卡，您可以設定 NPS 傳送和接收 RADIUS 流量只在指定的介面卡。

例如，不包含用 RADIUS 戶端網路區段可能會導致安裝在伺服器 NPS 一個網路介面卡時提供的第二個網路介面卡, 設定網路路徑 NPS RADIUS 戶端。 在本案例中，請務必直接 NPS 第二個的網路介面卡用於所有 RADIUS 傳輸。

在另一部範例中，如果 NPS 伺服器有三種網路介面卡，安裝，但您只想 NPS RADIUS 流量，使用的介面卡兩個您可以設定連接埠的兩個的介面卡的資訊。 藉由排除第三個配接器連接埠設定，您阻止 NPS RADIUS 流量使用介面卡。

## <a name="using-a-network-adapter"></a>使用網路介面卡

若要設定 NPS 接聽和傳送 RADIUS 流量網路介面卡，請使用下列語法」的網路原則伺服器中 NPS 主機上：

- IPv4 流量語法：IPAddress:UDPport 位置 IPAddress IPv4 位址設定您想要傳送 RADIUS 流量，透過這的網路介面卡，UDPport 且想要使用適用於 RADIUS 驗證或計量流量 RADIUS 連接埠號碼。
- IPv6 流量語法: [IPv6Address]: UDPport 位置括 IPv6Address 所需、IPv6Address IPv6 位址上您想要傳送 RADIUS 流量的網路介面卡設定並 UDPport 是您想要使用適用於 RADIUS 驗證或計量流量 RADIUS 連接埠號碼。

可以為分隔字元使用下列字元設定 IP 位址與 UDP 連接埠資訊：

- 地址埠分隔字元：分號（:）
- 連接埠分隔字元：逗號（，）
- 介面分隔字元：分號（;）

## <a name="configuring-network-access-servers"></a>設定網路存取伺服器

請確定您的網路存取伺服器已使用您設定在您 NPS 伺服器的相同 RADIUS UDP 連接埠號碼。 RADIUS 標準 UDP 連接埠 2865 年和 2866 Rfc 所述的驗證 1812 年和 1813，用於會計;不過，有些存取伺服器計量要求驗證要求 UDP 連接埠 1645 年與 UDP 連接埠 1646 年使用預設設定。

>[!IMPORTANT]
>如果您不使用 RADIUS 預設連接埠號碼，您必須設定例外防火牆允許上新的連接埠 RADIUS 流量本機電腦上。 如需詳細資訊，請查看[設定防火牆 RADIUS 流量的](nps-firewalls-configure.md)。

## <a name="configure-the-multihomed-nps-server"></a>設定多重主目錄 NPS 伺服器

您可以使用下列程序，設定多重主目錄 NPS 伺服器。

資格在**網域系統管理員**，或相當於，才能完成此程序最小值。

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>若要指定的網路介面卡和 NPS RADIUS 流量使用 UDP 連接埠

1. 在伺服器管理員中，按一下 [**工具**，然後按一下 [**的網路原則伺服器**打開 NPS 主機。

2. 以滑鼠右鍵按一下**的網路原則伺服器**，然後按一下 [**屬性**。

3. 按一下**連接埠**索引標籤，然後在名稱前面加上您想要使用的現有的連接埠號碼 RADIUS 流量之網路介面卡的 IP 位址。 例如，如果您想要使用的 IP 位址 192.168.1.2 和 RADIUS 連接埠 1812 年和 1645 年驗證要求，變更連接埠設定從**1812,1645**以**192.168.1.2:1812,1645**。 如果您的驗證 RADIUS 和 RADIUS 計量 UDP 連接埠不同的預設值，請變更的連接埠設定移動。

4. 驗證或計量要求使用多個連接埠設定，來以逗號分隔連接埠號碼。

如需 NPS UDP 連接埠，請查看[設定 NPS UDP 連接埠資訊](nps-udp-ports-configure.md)


如需 NPS 的詳細資訊，請查看[的網路原則伺服器](nps-top.md)


---
title: DHCP 子網路選取選項
description: 本主題提供適用於動態主機設定通訊協定 (DHCP) 在 Windows Server 2016 DHCP 子網路選取選項的相關資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 43bc3d165f895767ded921b41118ecaccf9734e8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="dhcp-subnet-selection-options"></a>DHCP 子網路選取選項

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題的新 DHCP 子網路選取選項的相關資訊。

DHCP 現在支援 \(sub-option 5\) 118 與 82 選項。 您可以使用這些選項來讓您要求的 IP 位址特定子網路，以及從指定 IP 位址和範圍 DHCP proxy 戶端及轉送代理程式。

如果您使用 DHCP proxy client 設定 DHCP 選項 118，例如執行 Windows Server 2016 和遠端存取伺服器角色 virtual 私人網路 (VPN) 伺服器、VPN 伺服器可以要求 IP 位址租用 VPN 戶端的特定的 IP 位址。

如果您使用 DHCP 轉送代理設定 DHCP 選項 82，子選項 5 轉接可以要求 IP 位址租用 DHCP 戶端的特定的 IP 位址。

以下是的意見主題這些選項要求的連結。

- **選項 118**: [RFC 3011 IPv4 子網路選取選項的 DHCP](http://www.rfc-base.org/rfc-3011.html)
- **選項 82 子選項 5**: [RFC 3527 連結選擇子選項轉送代理程式資訊選項 DHCPv4](https://tools.ietf.org/html/rfc3527)


## <a name="dhcp-option-118-client-subnet-and-relay-agent-link-selection"></a>DHCP 選項 118: Client 子網路和轉送代理連結-選取項目

DHCP 子網路選取選項提供機制 DHCP proxy，若要指定 IP 子網路，DHCP 伺服器應該指定 IP 位址和選項。

### <a name="use-case-scenario"></a>使用案例

在本案例中，virtual 私人網路 \(VPN\) 伺服器以 VPN 戶端配置的 IP 位址。 

在這個情況，VPN 伺服器已連接兩個內部 DHCP 伺服器安裝所在的網路和網際網路，以便 VPN 戶端可以連接的 VPN 伺服器從遠端位置。

VPN 伺服器 VPN 戶端可以提供 IP 位址租用之前，請伺服器連絡人內部子網路上的 DHCP 伺服器，並保留封鎖的 IP 位址。 VPN 伺服器然後管理它取得從 DHCP 伺服器的 IP 位址。 如果 VPN 伺服器提供所有的期保留 IP 位址 VPN 戶端，VPN 伺服器再從 DHCP 伺服器取得額外的 IP 位址。

藉由設定 VPN 伺服器 DHCP 選項 118，您可以指定 IP 位址範圍及您想要使用的 VPN 戶端範圍。 設定選項 118 之後，VPN 伺服器會要求特定 IP 位址，範圍從 DHCP 伺服器的 IP 位址。

### <a name="the-dhcp-subnet-selection-option-field"></a>[DHCP 子網路選擇選項] 欄位

[DHCP 子網路選擇選項] 欄位包含單一 IPv4 位址用來表示 DHCP 租賃要求的原始子網路位址。  設定此選項回應 DHCP 伺服器會從配置地址：

1. 子網路選取選項中指定子網路。
2. 上相同的網路區段子網路中子網路選取選項指定作為子網路。

## <a name="option-82-sub-option-5-link-selection-sub-option"></a>5 選項 82 子選項：連結子選項

轉送代理連結選取項目子選項可讓 DHCP 轉送代理指定 IP 子網路，DHCP 伺服器應該指定 IP 位址和選項。

通常 DHCP 轉送代理程式需依賴 \(GIADDR\) 閘道 IP 位址] 欄位來與 DHCP 伺服器通訊。 不過，GIADDR 受到其兩個操作功能：

1. 若要 DHCP 伺服器的 IP 位址租用要求 DHCP client 位於子網路通知。
2. 通知 DHCP 伺服器的 IP 位址，以用來與轉接通訊。

有時候，可能會不同的 IP 位址範圍需要配置 DHCP client IP 位址，轉接用來與 DHCP 伺服器的 IP 位址。 

使用功能有限現狀選項 118，並可以僅限 GIADDR 欄位或轉送代理程式資訊選項 \(option 82\) 寫入不能 DHCP 轉送代理程式。 

選項 82 連結選擇子選項適用於這種情形，允許轉接明確陳述子的網路，它想要的 IP 位址 DHCP v4 選項的形式 82 子選項 5 配置。

### <a name="use-case-scenario"></a>使用案例

在本案例中，組織網路包括 DHCP 伺服器和無線存取點的訪客使用者 \(AP\)。 從組織 DHCP 伺服器-不過，因為防火牆原則限制指派來賓 client IP 位址、DHCP 伺服器無法存取來賓 wireless 網路或 wireless 戶端 broadcase 訊息。

若要透過這項限制身分查驗，AP 設定是連結選擇子選項 5 指定子網路它想要從中來賓戶端，在也指定內部介面，導致公司網路的 IP 位址 GIADDR 中的配置的 IP 位址。

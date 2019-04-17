---
title: 設定防火牆 RADIUS 流量
description: 本主題提供如何設定 Windows Server 2016 中的網路原則伺服器允許 RADIUS 流量防火牆的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 140111e10eabbece098ae9b7c36746cc663c9cce
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-firewalls-for-radius-traffic"></a>設定防火牆 RADIUS 流量

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要允許或封鎖類型的電腦或裝置執行防火牆 IP 流量的可以設定防火牆。 如果防火牆設定不正確允許 RADIUS RADIUS 戶端間的流量，RADIUS proxy 和 RADIUS 伺服器、網路存取驗證可能會失敗，防止使用者存取網路資源。 

您可能需要設定防火牆允許 RADIUS 流量的兩種類型：

- Windows Defender 防火牆使用進階安全性本機伺服器執行的網路原則 Server (NPS)。
- 防火牆在其他電腦或裝置上執行。

## <a name="windows-firewall-on-the-local-nps-server"></a>Windows 防火牆 NPS 本機伺服器

根據預設，NPS 傳送和接收 RADIUS 流量透過 1812 年，1813 年、1645 年 1646 年的使用者資料流通訊協定 \(UDP\) 連接埠。 NPS 伺服器上的 Windows Defender 防火牆自動設定的例外，來傳送和接收此 RADIUS 流量 NPS，在安裝期間。

因此，如果您正在使用的預設 UDP 連接埠，您不需要變更允許的伺服器 NPS RADIUS 流量的 Windows Defender 防火牆設定。

有時候，您可能想要變更 NPS RADIUS 流量使用連接埠。 如果您設定 NPS 與您的網路存取權的伺服器來傳送和接收 RADIUS 流量連接埠以外的預設值，您必須執行下列動作：

- 移除例外允許 RADIUS 流量預設連接埠。
- 建立新例外允許 RADIUS 流量上新的連接埠。

如需詳細資訊，請查看[設定 NPS UDP 連接埠資訊](nps-udp-ports-configure.md)。

## <a name="other-firewalls"></a>其他防火牆

最常見的設定，防火牆已連接到網際網路，而且 NPS 伺服器的內部網路資源，已連接到周邊網路。

若要瑞曲之戰網域控制站內部，可能必須 NPS 伺服器：

- 在企業網路介面周邊網路上的介面（IP 路由不支援）。 
- 單一介面周邊網路上。 此設定，NPS 會使用透過其他周邊網路連接到網際網路防火牆網域控制站通訊。

## <a name="configuring-the-internet-firewall"></a>設定網際網路防火牆

已連接到網際網路防火牆必須使用網際網路介面的輸入與輸出篩選器設定 \（，然後選擇該網路周邊 interface\），以便在網際網路上轉送 RADIUS 伺服器 NPS RADIUS 戶端或 proxy 間的訊息。 其他篩選可用，讓傳流量的網頁伺服器、VPN 伺服器，以及其他類型的伺服器上周邊網路。

不同的篩選器可以在 [網際網路介面和周邊網路介面設定的輸入與輸出封包。

### <a name="configure-input-filters-on-the-internet-interface"></a>在 [網際網路介面設定輸入篩選器

允許下列類型的資料傳輸防火牆網際網路介面上，設定下列封的包篩選器：

- 目的地周邊網路介面和目的地連接埠 UDP 1812 (0x714) NPS 伺服器的 IP 位址。  此篩選會允許從網際網路 RADIUS 伺服器 NPS RADIUS 驗證資料傳輸。 這是定義 RFC 2865 NPS，使用的預設 UDP 連接埠。 如果您使用不同的連接埠，以替代 1812 年該連接埠號碼。
- 目的地周邊網路介面與 UDP 目的地連接埠 (0x715) 1813 NPS 伺服器的 IP 位址。 此篩選會允許從網際網路 RADIUS 伺服器 NPS RADIUS 計量流量。 這是定義 RFC 2866 NPS，使用的預設 UDP 連接埠。 如果您使用不同的連接埠，以替代 1813 年該連接埠號碼。
- \(Optional\) 周邊網路介面目的地 IP 位址和 UDP 目的地 1645 年 \(0x66D\) NPS 伺服器連接埠。 此篩選會允許從網際網路 RADIUS 伺服器 NPS RADIUS 驗證資料傳輸。 這是使用較舊 RADIUS 用 UDP 連接埠。
- \(Optional\) 周邊網路介面目的地 IP 位址和 UDP 目的地 1646 年 \(0x66E\) NPS 伺服器連接埠。 此篩選會允許從網際網路 RADIUS 伺服器 NPS RADIUS 計量流量。 這是使用較舊 RADIUS 用 UDP 連接埠。

### <a name="configure-output-filters-on-the-internet-interface"></a>在 [網際網路介面設定輸出篩選器

允許下列類型的資料傳輸防火牆網際網路介面上，設定下列輸出篩選器：

- 來源周邊網路介面與 UDP 來源連接埠 1812 (0x714) NPS 伺服器的 IP 位址。 此篩選允許 RADIUS 驗證流量 NPS 伺服器從網際網路 RADIUS 戶端。 這是定義 RFC 2865 NPS，使用的預設 UDP 連接埠。 如果您使用不同的連接埠，以替代 1812 年該連接埠號碼。
- 來源周邊網路介面與 UDP 來源連接埠 (0x715) 1813 NPS 伺服器的 IP 位址。 此篩選允許 RADIUS 計量流量 NPS 伺服器從網際網路 RADIUS 戶端。 這是定義 RFC 2866 NPS，使用的預設 UDP 連接埠。 如果您使用不同的連接埠，以替代 1813 年該連接埠號碼。
- \(Optional\) 周邊網路介面的來源 IP 位址和 UDP 來源 1645 年 \(0x66D\) NPS 伺服器連接埠。 此篩選允許 RADIUS 驗證流量 NPS 伺服器從網際網路 RADIUS 戶端。 這是使用較舊 RADIUS 用 UDP 連接埠。
- \(Optional\) 周邊網路介面的來源 IP 位址和 UDP 來源 1646 年 \(0x66E\) NPS 伺服器連接埠。 此篩選允許 RADIUS 計量流量 NPS 伺服器從網際網路 RADIUS 戶端。 這是使用較舊 RADIUS 用 UDP 連接埠。

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>設定輸入篩選器周邊網路介面

允許下列類型的資料傳輸防火牆周邊網路介面上，設定下列輸入篩選器：

- 來源周邊網路介面與 UDP 來源連接埠 1812 (0x714) NPS 伺服器的 IP 位址。 此篩選允許 RADIUS 驗證流量 NPS 伺服器從網際網路 RADIUS 戶端。 這是定義 RFC 2865 NPS，使用的預設 UDP 連接埠。 如果您使用不同的連接埠，以替代 1812 年該連接埠號碼。
- 來源周邊網路介面與 UDP 來源連接埠 (0x715) 1813 NPS 伺服器的 IP 位址。 此篩選允許 RADIUS 計量流量 NPS 伺服器從網際網路 RADIUS 戶端。 這是定義 RFC 2866 NPS，使用的預設 UDP 連接埠。 如果您使用不同的連接埠，以替代 1813 年該連接埠號碼。
- \(Optional\) 周邊網路介面的來源 IP 位址和 UDP 來源 1645 年 \(0x66D\) NPS 伺服器連接埠。 此篩選允許 RADIUS 驗證流量 NPS 伺服器從網際網路 RADIUS 戶端。 這是使用較舊 RADIUS 用 UDP 連接埠。
- \(Optional\) 周邊網路介面的來源 IP 位址和 UDP 來源 1646 年 \(0x66E\) NPS 伺服器連接埠。 此篩選允許 RADIUS 計量流量 NPS 伺服器從網際網路 RADIUS 戶端。 這是使用較舊 RADIUS 用 UDP 連接埠。

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>設定輸出篩選器周邊網路介面

允許下列類型的資料傳輸防火牆周邊網路介面上，設定下列輸出封包篩選器：

- 目的地周邊網路介面和目的地連接埠 UDP 1812 (0x714) NPS 伺服器的 IP 位址。 此篩選會允許從網際網路 RADIUS 伺服器 NPS RADIUS 驗證資料傳輸。 這是定義 RFC 2865 NPS，使用的預設 UDP 連接埠。 如果您使用不同的連接埠，以替代 1812 年該連接埠號碼。
- 目的地周邊網路介面與 UDP 目的地連接埠 (0x715) 1813 NPS 伺服器的 IP 位址。 此篩選會允許從網際網路 RADIUS 伺服器 NPS RADIUS 計量流量。 這是定義 RFC 2866 NPS，使用的預設 UDP 連接埠。 如果您使用不同的連接埠，以替代 1813 年該連接埠號碼。
- \(Optional\) 周邊網路介面目的地 IP 位址和 UDP 目的地 1645 年 \(0x66D\) NPS 伺服器連接埠。 此篩選會允許從網際網路 RADIUS 伺服器 NPS RADIUS 驗證資料傳輸。 這是使用較舊 RADIUS 用 UDP 連接埠。
- \(Optional\) 周邊網路介面目的地 IP 位址和 UDP 目的地 1646 年 \(0x66E\) NPS 伺服器連接埠。 此篩選會允許從網際網路 RADIUS 伺服器 NPS RADIUS 計量流量。 這是使用較舊 RADIUS 用 UDP 連接埠。

提高安全性，您可以使用每個 RADIUS client 傳送封包透過防火牆定義流量 client 之間 NPS 伺服器的 IP 位址篩選器周邊網路上的 IP 位址。

### <a name="filters-on-the-perimeter-network-interface"></a>周邊網路介面的篩選器

允許下列類型的資料傳輸內部防火牆周邊網路介面上，設定下列輸入封包篩選器：

- 來源周邊網路介面 NPS 伺服器的 IP 位址。 此篩選允許流量 NPS 伺服器從周邊網路上。

允許下列類型的資料傳輸內部防火牆周邊網路介面上，設定下列輸出篩選器：

- 目的地周邊網路介面 NPS 伺服器的 IP 位址。 此篩選允許周邊網路上的流量 NPS 伺服器。

### <a name="filters-on-the-intranet-interface"></a>在 internet 介面篩選

允許下列類型的資料傳輸在防火牆 internet 介面上，設定下列輸入篩選器：

- 目的地周邊網路介面 NPS 伺服器的 IP 位址。 此篩選允許周邊網路上的流量 NPS 伺服器。

允許下列類型的資料傳輸在防火牆 internet 介面上，設定下列輸出封包篩選器：

- 來源周邊網路介面 NPS 伺服器的 IP 位址。 此篩選允許流量 NPS 伺服器從周邊網路上。


如需有關管理 NPS 的詳細資訊，請查看[管理的網路原則伺服器]](nps-manage-top.md)。

如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。





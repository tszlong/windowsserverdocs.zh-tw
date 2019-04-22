---
title: 設定 RADIUS 流量的防火牆
description: 本主題提供如何設定防火牆以允許的 Windows Server 2016 中的網路原則伺服器 RADIUS 流量的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 94b03349f21a40f9bf42508d5a2878a5cf2946cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819449"
---
# <a name="configure-firewalls-for-radius-traffic"></a>設定 RADIUS 流量的防火牆

>適用於：Windows Server （半年通道），Windows Server 2016

可以設定防火牆來允許或封鎖從電腦或裝置執行防火牆的 IP 流量類型。 若防火牆未正確設定以允許 RADIUS 用戶端之間的 RADIUS 流量，RADIUS proxy 以及 RADIUS 伺服器的網路存取驗證可以失敗，導致使用者無法存取網路資源。 

您可能需要設定防火牆以允許 RADIUS 流量的兩種類型：

- 執行網路原則伺服器 (NPS) 的本機伺服器上具有進階安全性的 Windows Defender 防火牆。
- 在其他電腦或硬體裝置上執行的防火牆。

## <a name="windows-firewall-on-the-local-nps"></a>在本機 NPS 上的 Windows 防火牆

根據預設，NPS 會將傳送和接收 RADIUS 流量使用使用者資料包通訊協定\(UDP\)連接埠 1812年、 1813年、 1645年和 1646年。 在 NPS 中，以允許傳送和接收此 RADIUS 流量的安裝期間，會自動在 NPS 上的 Windows Defender 防火牆設定例外狀況下。

因此，如果您使用的預設 UDP 連接埠，您不需要變更 Windows Defender 防火牆設定，以允許從 NPSs 來回傳送 RADIUS 流量。

在某些情況下，您可能想要變更 NPS 用於 RADIUS 流量的連接埠。 如果您設定 NPS 與網路存取伺服器來傳送和接收 RADIUS 流量的預設值以外的連接埠，您必須執行下列作業：

- 移除允許預設連接埠的 RADIUS 流量的例外狀況。
- 建立新的例外狀況，允許新的連接埠上的 RADIUS 流量。

如需詳細資訊，請參閱 <<c0> [ 設定 NPS UDP 連接埠資訊](nps-udp-ports-configure.md)。

## <a name="other-firewalls"></a>其他防火牆

在最常見的組態中，防火牆連線到網際網路，以及 NPS 是連線到周邊網路的內部網路資源。

若要連線到內部網路中的網域控制站，可能有 NPS:

- 在周邊網路與內部網路上的介面上的介面 （IP 路由未啟用）。 
- 周邊網路上的單一介面。 在此組態中，NPS 會透過連接到內部網路的周邊網路的其他防火牆的網域控制站進行通訊。

## <a name="configuring-the-internet-firewall"></a>設定網際網路防火牆

連線到網際網路的防火牆必須設定其網際網路介面上的輸入與輸出篩選器\(和 （選擇性） 其網路周邊介面\)，以允許 NPS 之間的 RADIUS 訊息轉送和 RADIUS 用戶端或網際網路上的 proxy。 其他篩選器可用來允許將流量傳遞至 Web 伺服器、 VPN 伺服器，以及其他類型的伺服器在周邊網路上。

個別的網際網路介面與周邊網路介面，可以設定篩選器的輸入和輸出封包。

### <a name="configure-input-filters-on-the-internet-interface"></a>設定網際網路介面上的輸入篩選器

設定防火牆以允許下列類型的流量的網際網路介面上的下列輸入封包篩選器：

- 周邊網路介面和 NPS 的 1812 (0x714) UDP 目的地連接埠的目的地 IP 位址。  此篩選器允許從網際網路為主的 RADIUS 用戶端至 NPS 的 RADIUS 驗證流量。 這是 NPS，使用的預設 UDP 連接埠，如 RFC 2865 中所定義。 如果您使用不同的連接埠，以該連接埠號碼取代 1812年。
- 周邊網路介面和 NPS 的 1813 (0x715) UDP 目的地連接埠的目的地 IP 位址。 此篩選器會允許從網際網路為主的 RADIUS 用戶端到 NPS RADIUS 帳戶處理流量。 這是 NPS，使用的預設 UDP 連接埠，如 RFC 2866 中所定義。 如果您使用不同的連接埠，以取代該通訊埠編號 1813年。
- \(選擇性\)UDP 目的地連接埠 1645年與周邊網路介面的目的地 IP 位址\(0x66D\)的 NPS。 此篩選器允許從網際網路為主的 RADIUS 用戶端至 NPS 的 RADIUS 驗證流量。 這是較舊的 RADIUS 用戶端會使用 UDP 連接埠。
- \(選擇性\)UDP 目的地連接埠 1646年與周邊網路介面的目的地 IP 位址\(0x66E\)的 NPS。 此篩選器會允許從網際網路為主的 RADIUS 用戶端到 NPS RADIUS 帳戶處理流量。 這是較舊的 RADIUS 用戶端會使用 UDP 連接埠。

### <a name="configure-output-filters-on-the-internet-interface"></a>設定網際網路介面上的輸出篩選器

設定防火牆以允許下列類型的流量的網際網路介面上的下列輸出篩選器：

- 來源 IP 位址的周邊網路介面和 NPS 的 1812 (0x714) UDP 來源連接埠。 此篩選器允許從 NPS 到以網際網路為基礎的 RADIUS 用戶端的 RADIUS 驗證流量。 這是 NPS，使用的預設 UDP 連接埠，如 RFC 2865 中所定義。 如果您使用不同的連接埠，以該連接埠號碼取代 1812年。
- 來源 IP 位址的周邊網路介面和 NPS 的 1813 (0x715) UDP 來源連接埠。 此篩選會允許從 NPS RADIUS 帳戶處理流量，到以網際網路為基礎的 RADIUS 用戶端。 這是 NPS，使用的預設 UDP 連接埠，如 RFC 2866 中所定義。 如果您使用不同的連接埠，以取代該通訊埠編號 1813年。
- \(選擇性\)UDP 來源連接埠 1645年與周邊網路介面的來源 IP 位址\(0x66D\)的 NPS。 此篩選器允許從 NPS 到以網際網路為基礎的 RADIUS 用戶端的 RADIUS 驗證流量。 這是較舊的 RADIUS 用戶端會使用 UDP 連接埠。
- \(選擇性\)來源 IP 位址的周邊網路介面和 UDP 來源連接埠 1646年\(0x66E\)的 NPS。 此篩選會允許從 NPS RADIUS 帳戶處理流量，到以網際網路為基礎的 RADIUS 用戶端。 這是較舊的 RADIUS 用戶端會使用 UDP 連接埠。

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>周邊網路介面上設定輸入篩選器

設定防火牆以允許下列類型的流量的周邊網路介面上的下列輸入篩選器：

- 來源 IP 位址的周邊網路介面和 NPS 的 1812 (0x714) UDP 來源連接埠。 此篩選器允許從 NPS 到以網際網路為基礎的 RADIUS 用戶端的 RADIUS 驗證流量。 這是 NPS，使用的預設 UDP 連接埠，如 RFC 2865 中所定義。 如果您使用不同的連接埠，以該連接埠號碼取代 1812年。
- 來源 IP 位址的周邊網路介面和 NPS 的 1813 (0x715) UDP 來源連接埠。 此篩選會允許從 NPS RADIUS 帳戶處理流量，到以網際網路為基礎的 RADIUS 用戶端。 這是 NPS，使用的預設 UDP 連接埠，如 RFC 2866 中所定義。 如果您使用不同的連接埠，以取代該通訊埠編號 1813年。
- \(選擇性\)UDP 來源連接埠 1645年與周邊網路介面的來源 IP 位址\(0x66D\)的 NPS。 此篩選器允許從 NPS 到以網際網路為基礎的 RADIUS 用戶端的 RADIUS 驗證流量。 這是較舊的 RADIUS 用戶端會使用 UDP 連接埠。
- \(選擇性\)來源 IP 位址的周邊網路介面和 UDP 來源連接埠 1646年\(0x66E\)的 NPS。 此篩選會允許從 NPS RADIUS 帳戶處理流量，到以網際網路為基礎的 RADIUS 用戶端。 這是較舊的 RADIUS 用戶端會使用 UDP 連接埠。

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>周邊網路介面上設定輸出篩選器

設定防火牆以允許下列類型的流量的周邊網路介面上的下列輸出封包篩選器：

- 周邊網路介面和 NPS 的 1812 (0x714) UDP 目的地連接埠的目的地 IP 位址。 此篩選器允許從網際網路為主的 RADIUS 用戶端至 NPS 的 RADIUS 驗證流量。 這是 NPS，使用的預設 UDP 連接埠，如 RFC 2865 中所定義。 如果您使用不同的連接埠，以該連接埠號碼取代 1812年。
- 周邊網路介面和 NPS 的 1813 (0x715) UDP 目的地連接埠的目的地 IP 位址。 此篩選器會允許從網際網路為主的 RADIUS 用戶端到 NPS RADIUS 帳戶處理流量。 這是 NPS，使用的預設 UDP 連接埠，如 RFC 2866 中所定義。 如果您使用不同的連接埠，以取代該通訊埠編號 1813年。
- \(選擇性\)UDP 目的地連接埠 1645年與周邊網路介面的目的地 IP 位址\(0x66D\)的 NPS。 此篩選器允許從網際網路為主的 RADIUS 用戶端至 NPS 的 RADIUS 驗證流量。 這是較舊的 RADIUS 用戶端會使用 UDP 連接埠。
- \(選擇性\)UDP 目的地連接埠 1646年與周邊網路介面的目的地 IP 位址\(0x66E\)的 NPS。 此篩選器會允許從網際網路為主的 RADIUS 用戶端到 NPS RADIUS 帳戶處理流量。 這是較舊的 RADIUS 用戶端會使用 UDP 連接埠。

為了提高安全性，您可以使用每個 RADIUS 用戶端傳送的封包通過防火牆周邊網路上，定義用戶端與 NPS 的 IP 位址之間的流量篩選器的 IP 位址。

### <a name="filters-on-the-perimeter-network-interface"></a>周邊網路介面上的篩選器

在內部網路防火牆以允許下列類型的流量的周邊網路介面上設定下列輸入封包篩選器：

- NPS 的周邊網路介面的來源 IP 位址。 此篩選器允許流量從周邊網路上的 NPS。

在內部網路防火牆以允許下列類型的流量的周邊網路介面上設定下列輸出篩選器：

- NPS 的周邊網路介面的目的地 IP 位址。 此篩選器允許周邊網路上的流量給 NPS。

### <a name="filters-on-the-intranet-interface"></a>內部網路介面的篩選器

設定防火牆以允許下列類型的流量的內部網路介面上的下列輸入篩選器：

- NPS 的周邊網路介面的目的地 IP 位址。 此篩選器允許周邊網路上的流量給 NPS。

下列的輸出封包篩選器的介面上設定內部網路的防火牆，以允許下列類型的流量：

- NPS 的周邊網路介面的來源 IP 位址。 此篩選器允許流量從周邊網路上的 NPS。


如需管理 NPS 的詳細資訊，請參閱 <<c0> [ 管理網路原則伺服器](nps-manage-top.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS)](nps-top.md)。





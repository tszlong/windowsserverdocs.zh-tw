---
title: 設定 RADIUS 流量的防火牆
description: 本主題概要說明如何設定防火牆，以允許 Windows Server 2016 中網路原則伺服器的 RADIUS 流量。
manager: brianlic
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: e9c89312c267d2344ecfc3b48a5f6e050c353093
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946814"
---
# <a name="configure-firewalls-for-radius-traffic"></a>設定 RADIUS 流量的防火牆

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以針對執行防火牆的電腦或裝置，設定防火牆所要允許或封鎖的送入或送出 IP 流量類型。 如果未正確設定防火牆以允許 RADIUS 用戶端、RADIUS Proxy 以及 RADIUS 伺服器之間的 RADIUS 流量，網路存取驗證可能會失敗，而導致使用者無法存取網路資源。

您可能需要設定兩種類型的防火牆，以允許 RADIUS 流量：

- 在執行網路原則伺服器 (NPS) 的本機伺服器上 Windows Defender 防火牆具有 Advanced 安全性。
- 在其他電腦或硬體裝置上執行的防火牆。

## <a name="windows-firewall-on-the-local-nps"></a>本機 NPS 上的 Windows 防火牆

NPS 預設會使用使用者資料包協定 \( UDP \) 埠1812、1813、1645和1646來傳送和接收 RADIUS 流量。 NPS 上的 Windows Defender 防火牆會在 NPS 安裝期間自動設定例外狀況，以允許傳送和接收此 RADIUS 流量。

因此，如果您使用預設 UDP 埠，則不需要變更 Windows Defender 防火牆設定，以允許 NPSs 的 RADIUS 流量。

在某些情況下，您可能會想要變更 NPS 用於 RADIUS 流量的連接埠。 如果您設定 NPS 與網路存取伺服器使用預設值以外的連接埠傳送和接收 RADIUS 流量，就必須執行下列動作：

- 移除允許在預設埠上使用 RADIUS 流量的例外狀況。
- 建立新的例外狀況，以允許新埠上的 RADIUS 流量。

如需詳細資訊，請參閱 [設定 NPS UDP 埠資訊](nps-udp-ports-configure.md)。

## <a name="other-firewalls"></a>其他防火牆

在最常見的設定中，防火牆會連接到網際網路，而 NPS 是連線到周邊網路的內部網路資源。

若要連接到內部網路中的網域控制站，NPS 可能具有：

- 一個周邊網路介面以及一個內部網路介面 (未啟用 IP 路由)。
- 一個單一的周邊網路介面。 在此設定中，NPS 會透過將周邊網路連接到內部網路的另一個防火牆，與網域控制站通訊。

## <a name="configuring-the-internet-firewall"></a>設定網際網路防火牆

連線到網際網路的防火牆必須在其網際網路介面上設定輸入和輸出篩選器， \( 也可以選擇其網路周邊介面 \) ，以允許在 NPS 與 radius 用戶端或網際網路上的 proxy 之間轉送 RADIUS 訊息。 其他篩選器可用來允許將流量傳遞到網頁伺服器、VPN 伺服器，以及周邊網路上其他類型的伺服器。

您可以在網際網路介面與周邊網路介面上，設定個別的輸入與輸出封包篩選器。

### <a name="configure-input-filters-on-the-internet-interface"></a>在網際網路介面上設定輸入篩選

在防火牆的網際網路介面上設定下列輸入封包篩選器，即可允許下列類型的流量：

- 周邊網路介面的目的地 IP 位址和 1812 (0x714) 的 UDP 目的地埠。  此篩選器可讓您從以網際網路為基礎的 RADIUS 用戶端到 NPS 的 RADIUS 驗證流量。 這是 NPS 使用的預設 UDP 埠，如 RFC 2865 中所定義。 如果您使用不同的埠，請以該埠號碼取代1812。
- 周邊網路介面的目的地 IP 位址和 1813 (0x715) 的 UDP 目的地埠。 此篩選器可讓您從以網際網路為基礎的 RADIUS 用戶端到 NPS 的 RADIUS 帳戶處理流量。 這是 NPS 使用的預設 UDP 埠，如 RFC 2866 中所定義。 如果您使用不同的埠，請以該埠號碼取代1813。
- \(\)周邊網路介面的選擇性目的地 IP 位址，以及 NPS 1645 0x66D 的 UDP 目的地埠 \( \) 。 此篩選器可讓您從以網際網路為基礎的 RADIUS 用戶端到 NPS 的 RADIUS 驗證流量。 這是較舊的 RADIUS 用戶端所使用的 UDP 連接埠。
- \(\)周邊網路介面的選擇性目的地 IP 位址，以及 NPS 1646 0x66E 的 UDP 目的地埠 \( \) 。 此篩選器可讓您從以網際網路為基礎的 RADIUS 用戶端到 NPS 的 RADIUS 帳戶處理流量。 這是較舊的 RADIUS 用戶端所使用的 UDP 連接埠。

### <a name="configure-output-filters-on-the-internet-interface"></a>在網際網路介面上設定輸出篩選

在防火牆的網際網路介面上設定下列輸出篩選器，即可允許下列類型的流量：

- 周邊網路介面的來源 IP 位址和1812的 UDP 來源埠 (0x714) 的 NPS。 此篩選器允許從 NPS 到以網際網路為基礎的 RADIUS 用戶端的 RADIUS 驗證流量。 這是 NPS 使用的預設 UDP 埠，如 RFC 2865 中所定義。 如果您使用不同的埠，請以該埠號碼取代1812。
- 周邊網路介面的來源 IP 位址和1813的 UDP 來源埠 (0x715) 的 NPS。 此篩選器允許從 NPS 到以網際網路為基礎的 RADIUS 用戶端的 RADIUS 帳戶處理流量。 這是 NPS 使用的預設 UDP 埠，如 RFC 2866 中所定義。 如果您使用不同的埠，請以該埠號碼取代1813。
- \(\)周邊網路介面的選擇性來源 IP 位址，以及 NPS 1645 0x66D 的 UDP 來源埠 \( \) 。 此篩選器允許從 NPS 到以網際網路為基礎的 RADIUS 用戶端的 RADIUS 驗證流量。 這是較舊的 RADIUS 用戶端所使用的 UDP 連接埠。
- \(\)周邊網路介面的選擇性來源 IP 位址，以及 NPS 1646 0x66E 的 UDP 來源埠 \( \) 。 此篩選器允許從 NPS 到以網際網路為基礎的 RADIUS 用戶端的 RADIUS 帳戶處理流量。 這是較舊的 RADIUS 用戶端所使用的 UDP 連接埠。

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>設定周邊網路介面上的輸入篩選

在防火牆的周邊網路介面上設定下列輸入篩選器，即可允許下列類型的流量：

- 周邊網路介面的來源 IP 位址和1812的 UDP 來源埠 (0x714) 的 NPS。 此篩選器允許從 NPS 到以網際網路為基礎的 RADIUS 用戶端的 RADIUS 驗證流量。 這是 NPS 使用的預設 UDP 埠，如 RFC 2865 中所定義。 如果您使用不同的埠，請以該埠號碼取代1812。
- 周邊網路介面的來源 IP 位址和1813的 UDP 來源埠 (0x715) 的 NPS。 此篩選器允許從 NPS 到以網際網路為基礎的 RADIUS 用戶端的 RADIUS 帳戶處理流量。 這是 NPS 使用的預設 UDP 埠，如 RFC 2866 中所定義。 如果您使用不同的埠，請以該埠號碼取代1813。
- \(\)周邊網路介面的選擇性來源 IP 位址，以及 NPS 1645 0x66D 的 UDP 來源埠 \( \) 。 此篩選器允許從 NPS 到以網際網路為基礎的 RADIUS 用戶端的 RADIUS 驗證流量。 這是較舊的 RADIUS 用戶端所使用的 UDP 連接埠。
- \(\)周邊網路介面的選擇性來源 IP 位址，以及 NPS 1646 0x66E 的 UDP 來源埠 \( \) 。 此篩選器允許從 NPS 到以網際網路為基礎的 RADIUS 用戶端的 RADIUS 帳戶處理流量。 這是較舊的 RADIUS 用戶端所使用的 UDP 連接埠。

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>在周邊網路介面上設定輸出篩選

在防火牆的周邊網路介面上設定下列輸出封包篩選器，即可允許下列類型的流量：

- 周邊網路介面的目的地 IP 位址和 1812 (0x714) 的 UDP 目的地埠。 此篩選器可讓您從以網際網路為基礎的 RADIUS 用戶端到 NPS 的 RADIUS 驗證流量。 這是 NPS 使用的預設 UDP 埠，如 RFC 2865 中所定義。 如果您使用不同的埠，請以該埠號碼取代1812。
- 周邊網路介面的目的地 IP 位址和 1813 (0x715) 的 UDP 目的地埠。 此篩選器可讓您從以網際網路為基礎的 RADIUS 用戶端到 NPS 的 RADIUS 帳戶處理流量。 這是 NPS 使用的預設 UDP 埠，如 RFC 2866 中所定義。 如果您使用不同的埠，請以該埠號碼取代1813。
- \(\)周邊網路介面的選擇性目的地 IP 位址，以及 NPS 1645 0x66D 的 UDP 目的地埠 \( \) 。 此篩選器可讓您從以網際網路為基礎的 RADIUS 用戶端到 NPS 的 RADIUS 驗證流量。 這是較舊的 RADIUS 用戶端所使用的 UDP 連接埠。
- \(\)周邊網路介面的選擇性目的地 IP 位址，以及 NPS 1646 0x66E 的 UDP 目的地埠 \( \) 。 此篩選器可讓您從以網際網路為基礎的 RADIUS 用戶端到 NPS 的 RADIUS 帳戶處理流量。 這是較舊的 RADIUS 用戶端所使用的 UDP 連接埠。

為了增加安全性，您可以使用每個 RADIUS 用戶端的 IP 位址，透過防火牆傳送封包，來定義用戶端與周邊網路上 NPS IP 位址之間的流量篩選。

### <a name="filters-on-the-perimeter-network-interface"></a>周邊網路介面上的篩選器

在內部網路防火牆的周邊網路介面上設定下列輸入封包篩選器，即可允許下列類型的流量：

- NPS 周邊網路介面的來源 IP 位址。 此篩選器允許來自周邊網路上 NPS 的流量。

在內部網路防火牆的周邊網路介面上設定下列輸出篩選器，即可允許下列類型的流量：

- NPS 周邊網路介面的目的地 IP 位址。 此篩選器允許周邊網路上 NPS 的流量。

### <a name="filters-on-the-intranet-interface"></a>內部網路介面上的篩選器

在防火牆的內部網路介面上設定下列輸入篩選器，即可允許下列類型的流量：

- NPS 周邊網路介面的目的地 IP 位址。 此篩選器允許周邊網路上 NPS 的流量。

在防火牆的內部網路介面上設定下列輸出封包篩選器，即可允許下列類型的流量：

- NPS 周邊網路介面的來源 IP 位址。 此篩選器允許來自周邊網路上 NPS 的流量。


如需有關管理 NPS 的詳細資訊，請參閱 [管理網路原則伺服器](nps-manage-top.md)。

如需有關 NPS 的詳細資訊，請參閱 [網路原則伺服器 (nps) ](nps-top.md)。





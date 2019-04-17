---
title: 設定 DNS 和防火牆設定
description: 本主題提供部署 Always On VPN Windows Server 2016 中的詳細的指示。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/11/2018
ms.openlocfilehash: f57bfa005ef1af2556e4bd1cbb90ef8d3cfd22e3
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067129"
---
# 步驟 5. 設定 DNS 和防火牆設定

>適用於： Windows Server （半年度管道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #171; [**先前：** 步驟 4。安裝與設定 NPS 伺服器](vpn-deploy-nps.md)<br>
& #187; [**下一步：** 步驟 6。一律在 VPN 連線設定 Windows 10 用戶端](vpn-deploy-client-vpn-connections.md)

在此步驟中，您可以設定 DNS 及防火牆適用於 VPN 連線設定。

## 設定 DNS 名稱解析

在遠端 VPN 用戶端連線時，它們會使用您的內部用戶端所使用的相同 DNS 伺服器，可讓他們能夠解析名稱做為您的內部工作站的其餘部分的相同方式集。 

基於這個原因，您必須確定外部用戶端用來連線到 VPN 伺服器的電腦名稱符合憑證發給 VPN 伺服器中定義的主體替代名稱。

若要確保遠端用戶端可以連線到 VPN 伺服器，您可以建立 DNS A （主機） 記錄您的外部 DNS 區域中。 A 記錄應該使用 VPN 伺服器的憑證主體替代名稱。


### 若要新增主機 \ （A 或 AAAA\） 資源記錄至區域

1. DNS 伺服器上，在伺服器管理員中，按一下**工具**]，然後按一下 [ **DNS**。 開啟 DNS 管理員。
2. 在 DNS 管理員] 主控台樹狀目錄中，按一下您想要管理的伺服器。
3. 在詳細資料窗格中，在 [**名稱**] 以展開檢視 double\ 按一下**正向對應區域**。
4. **正向對應區域**的詳細資訊，right\ 按一下正向對應區域中以您想要新增的記錄，然後按一下**新的主機 \(A or AAAA\)**。 **新的主機**對話方塊隨即開啟。
5. 在**新的主機**，在**名稱**中，輸入 VPN 伺服器的憑證主體替代名稱。
6. IP 位址在輸入 VPN 伺服器的 IP 位址。 您可以輸入的地址中第 4 版 (IPv4) IP 格式，以新增主機 \(A\) 資源記錄或新增主機 \(AAAA\) 資源記錄的 IP 第 6 版 \(IPv6\) 格式。
7. 如果您已建立反向對應區域的 IP 位址範圍，包括 IP 位址在輸入時，然後選取 [**建立關聯的指標 (PTR) 記錄**核取方塊。  選取這個選項會建立的其他指標 \(PTR\) 資源記錄反向區域中針對此主機，根據您輸入的**名稱**和**IP 位址**的資訊。
8. 按一下 [**新增主機**]。

## 設定 Edge 防火牆

Edge 防火牆從公用網際網路分隔外部適用於周邊網路。 這個區隔的視覺化呈現方式，請參閱主題[一律在 VPN 技術概觀](../always-on-vpn-technology-overview.md)中的圖例。

必須允許的邊緣防火牆，並將它轉送到您的 VPN 伺服器的特定連接埠。 如果您使用網路位址轉譯 \(NAT\) edge 防火牆上時，您可能需要啟用使用者資料包通訊協定 \(UDP\) ports500 和 4500 連接埠轉送。 轉送到 IP 位址指派給您的 VPN 伺服器的外部介面這些連接埠。

如果您正在路由流量輸入，並執行 NAT 或背後的 VPN 伺服器，則您必須開啟您的防火牆規則，以允許 UDP ports500 並 4500 輸入至外部 IP 位址套用到 VPN 伺服器上的公用介面。

在任一情況下，如果您的防火牆支援深層的封包檢查，而您遇到困難，建立用戶端連線，您應該嘗試放鬆或停用深度的封包檢查 IKE 工作階段。

如需如何進行這些設定變更的資訊，請參閱您的防火牆文件。

## 設定內部周邊網路防火牆

內部周邊網路防火牆分隔從內部周邊網路的組織/公司網路。 這個區隔的視覺化呈現方式，請參閱主題[一律在 VPN 技術概觀](../always-on-vpn-technology-overview.md)中的圖例。

在此部署中，適用於周邊網路上的遠端存取 VPN 伺服器設定為 RADIUS 用戶端。  VPN 伺服器會 RADIUS 流量傳送到公司網路上 NPS，然後也會收到來自 NPS 的 RADIUS 流量。

設定防火牆以允許在兩個方向的 RADIUS 流量。


>[!NOTE]
>作為 RADIUS 伺服器的 VPN 伺服器，也就是 RADIUS 用戶端 NPS 伺服器上的組織/公司網路函式。 如需有關 RADIUS 基礎結構的詳細資訊，請參閱[網路原則伺服器 (NPS)](../../../../../networking/technologies/nps/nps-top.md)。

### RADIUS 流量的連接埠上的 VPN 伺服器和 NPS 伺服器

根據預設，NPS 和 VPN 接聽的連接埠 1812年、 1813年、 1645年和 1646年上所有已安裝的網路介面卡上的 RADIUS 流量。 如果您啟用具有進階安全性的 Windows 防火牆安裝 NPS 時，這些連接埠的防火牆例外狀況取得 IPv4 和 IPv6 流量的安裝程序期間自動建立。

>[!IMPORTANT]
>如果您的網路存取伺服器設定為將 RADIUS 流量傳送這些預設值以外的連接埠，移除 NPS 在安裝期間，建立具有進階安全性的 Windows 防火牆中的例外狀況，並建立適用於您使用的連接埠的例外狀況RADIUS 流量。

### 內部周邊網路防火牆設定為使用相同的半徑連接埠

如果您使用預設 RADIUS 連接埠設定 VPN 伺服器和 NPS 伺服器上，請確定您開啟下列在內部周邊網路防火牆連接埠：

- 連接埠 UDP1812、 UDP1813、 UDP1645，以及 UDP1646

如果您未使用的預設半徑連接埠 NPS 部署中，您必須設定防火牆，允許您正在使用的連接埠上的 RADIUS 流量。 如需詳細資訊，請參閱[設定 RADIUS 流量的防火牆](../../../../../networking/technologies/nps/nps-firewalls-configure.md)。

## 後續步驟
[步驟 6。設定 Windows 10 用戶端一律在 VPN 連線](vpn-deploy-client-vpn-connections.md)： 在此步驟中，您可以設定 Windows 10 用戶端電腦與 VPN 連線使用該基礎結構進行通訊。 您可以使用數種技術來設定 Windows 10 VPN 用戶端，包括 Windows PowerShell、 System Center Configuration Manager 和 Intune。 所有三個需要設定適當的 VPN 設定 XML VPN 設定檔。 

---

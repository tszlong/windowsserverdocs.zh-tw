---
title: 設定 DNS 和防火牆設定
description: 本主題提供部署一律開啟 」 VPN Windows Server 2016 中的詳細的的指示。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/11/2018
ms.openlocfilehash: f57bfa005ef1af2556e4bd1cbb90ef8d3cfd22e3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886249"
---
# <a name="step-5-configure-dns-and-firewall-settings"></a>步驟 5. 設定 DNS 和防火牆設定

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows 10

&#171;  [**前一個：** 步驟 4.安裝和設定 NPS 伺服器](vpn-deploy-nps.md)<br>
&#187;  [**下一步]:** 步驟 6.設定 [Windows 10 用戶端一律開啟 VPN 連線](vpn-deploy-client-vpn-connections.md)

在此步驟中，您設定 DNS 和防火牆設定 VPN 連線能力。

## <a name="configure-dns-name-resolution"></a>設定 DNS 名稱解析

當遠端 VPN 用戶端連線時，它們會使用您的內部用戶端使用，同一個 DNS 伺服器，以便讓它們以在您的內部工作站的其餘部分相同的方式來解析名稱集。 

基於這個原因，您必須確定外部的用戶端用來連線至 VPN 伺服器的電腦名稱比對到 VPN 伺服器所發出的憑證中定義的主體替代名稱。

若要確保遠端用戶端可以連線到 VPN 伺服器，您可以在外部 DNS 區域中建立 DNS A （主機） 記錄。 A 記錄應該使用 VPN 伺服器的憑證主體替代名稱。


### <a name="to-add-a-host-a-or-aaaa-resource-record-to-a-zone"></a>若要新增的主機\(A 或 AAAA\)區域資源記錄

1. DNS 伺服器上，在 [伺服器管理員] 中，按一下**工具**，然後按一下**DNS**。 此時會開啟 [DNS 管理員] 中。
2. 在 DNS 管理員主控台樹狀目錄中，按一下您想要管理的伺服器。
3. 在 詳細資料 窗格中，在**名稱**、 雙精度浮點\-按一下 **正向對應區域**展開檢視。
4. 在 **正向對應區域**詳細資料，右邊\-按一下您想要新增記錄、，然後按一下正向對應區域**新主機\(A 或 AAAA\)**。 **新的主控件**對話方塊隨即開啟。
5. 在 **新的主機**，請在**名稱**，輸入 VPN 伺服器的憑證主體替代名稱。
6. 在 [IP 位址] 中，輸入 VPN 伺服器的 IP 位址。 您可以輸入 IP 第 4 版 (IPv4) 位址格式，以將主機新增\(A\)資源記錄或 IP 第 6 版\(IPv6\)格式，以將主機新增\(AAAA\)資源記錄。
7. 如果您已建立反向對應區域的 IP 位址範圍，包括 IP 位址，您輸入，然後選取**建立關聯的指標 (PTR) 記錄**核取方塊。  選取此選項會建立額外的指標\(PTR\)資源記錄，反向區域為此主控件，資訊中所輸入**名稱**並**IP 位址**。
8. 按一下 [新增主機] 。

## <a name="configure-the-edge-firewall"></a>設定邊緣防火牆

邊緣防火牆區隔外部的周邊網路，從公用網際網路。 這種區隔的視覺表示法，請參閱主題中的圖例[一律在 VPN 技術概觀](../always-on-vpn-technology-overview.md)。

邊緣防火牆必須允許並轉送至 VPN 伺服器的特定連接埠。 如果您使用網路位址轉譯\(NAT\)在邊緣防火牆中，您可能需要啟用的使用者資料包通訊協定的連接埠轉送\(UDP\)連接埠 500 和 4500。 這些連接埠轉送至指派給您的 VPN 伺服器的外部介面的 IP 位址。

如果您要路由傳送流量的輸入，並執行 NAT 或背後 VPN 伺服器，則您必須開啟您的防火牆規則以允許 UDP 連接埠 500 和 4500 輸入套用到 VPN 伺服器上的公用介面的外部 IP 位址。

在任一情況下，如果您的防火牆支援深度封包檢查，而且您有困難，建立用戶端連線，您應該嘗試放寬或停用 IKE 工作階段的深度封包檢查。

如需如何進行這些設定變更的資訊，請參閱防火牆文件。

## <a name="configure-the-internal-perimeter-network-firewall"></a>設定內部的周邊網路防火牆

內部的周邊網路防火牆會分開內部的周邊網路中的組織/公司網路。 這種區隔的視覺表示法，請參閱主題中的圖例[一律在 VPN 技術概觀](../always-on-vpn-technology-overview.md)。

在此部署中，周邊網路上的遠端存取 VPN 伺服器被設定為 RADIUS 用戶端。  VPN 伺服器會傳送到公司網路上 NPS 的 RADIUS 流量，並也會從 NPS 收到 RADIUS 流量。

設定防火牆以允許流向兩個方向的 RADIUS 流量。


>[!NOTE]
>NPS 的伺服器上的組織/公司網路函式做為 VPN 伺服器，也就是 RADIUS 用戶端的 RADIUS 伺服器。 如需 RADIUS 基礎結構的詳細資訊，請參閱[網路原則伺服器 (NPS)](../../../../../networking/technologies/nps/nps-top.md)。

### <a name="radius-traffic-ports-on-the-vpn-server-and-nps-server"></a>RADIUS 流量的連接埠上的 VPN 伺服器和 NPS 伺服器

根據預設，NPS 與 VPN 接聽的連接埠 1812年、 1813年、 1645年和 1646年上所有已安裝的網路介面卡上的 RADIUS 流量。 如果您在安裝 NPS 時啟用具有進階安全性的 Windows 防火牆，這些連接埠的防火牆例外狀況取得 IPv6 和 IPv4 流量的安裝程序期間自動建立。

>[!IMPORTANT]
>如果您的網路存取伺服器會設定為透過非預設的連接埠傳送 RADIUS 流量，移除 NPS 在安裝期間，建立具有進階安全性的 Windows 防火牆中的例外狀況，並建立您要針對使用的連接埠的例外狀況RADIUS 流量。

### <a name="use-the-same-radius-ports-for-the-internal-perimeter-network-firewall-configuration"></a>使用相同的 RADIUS 連接埠內部的周邊網路防火牆組態

如果您在 VPN 伺服器與 NPS 伺服器上使用預設的 RADIUS 連接埠組態，請確定您開啟下列連接埠內部的周邊網路防火牆上：

- 連接埠 UDP1812、 UDP1813、 UDP1645 和 UDP1646

如果您不會使用預設的 RADIUS 連接埠 NPS 部署中，您必須設定防火牆以允許您使用的連接埠上的 RADIUS 流量。 如需詳細資訊，請參閱 <<c0> [ 設定用於 RADIUS 流量的防火牆](../../../../../networking/technologies/nps/nps-firewalls-configure.md)。

## <a name="next-step"></a>後續步驟
[步驟 6。設定 Windows 10 用戶端一律開啟 VPN 連線](vpn-deploy-client-vpn-connections.md):在此步驟中，您可以設定 Windows 10 用戶端電腦通訊的 VPN 連線與該基礎結構。 若要設定 Windows 10 VPN 用戶端，包括 Windows PowerShell、 System Center Configuration Manager 和 Intune，您可以使用數種技術。 這三個需要 XML VPN 設定檔來設定適當的 VPN 設定。 

---

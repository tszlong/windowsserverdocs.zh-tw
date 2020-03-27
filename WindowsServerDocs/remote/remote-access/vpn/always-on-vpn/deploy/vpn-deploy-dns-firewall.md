---
title: 設定 DNS 和防火牆設定
description: 本主題提供在 Windows Server 2016 中部署 Always On VPN 的詳細指示。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: lizross
author: eross-msft
ms.date: 06/11/2018
ms.openlocfilehash: 394e589028d9d3d22851ea970346b0f150fee393
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309400"
---
# <a name="step-5-configure-dns-and-firewall-settings"></a>步驟 5. 設定 DNS 和防火牆設定

>適用于： Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows 10

- [**上一步：** 步驟4。安裝和設定 NPS 伺服器](vpn-deploy-nps.md)
- [**下一步：** 步驟6。設定 Windows 10 用戶端 Always On VPN 連線](vpn-deploy-client-vpn-connections.md)

在此步驟中，您會設定 VPN 連線能力的 DNS 和防火牆設定。

## <a name="configure-dns-name-resolution"></a>設定 DNS 名稱解析

當遠端 VPN 用戶端連線時，它們會使用您的內部用戶端所使用的相同 DNS 伺服器，讓它們能夠以與您的其他內部工作站相同的方式來解析名稱。

因此，您必須確定外部用戶端用來連線到 VPN 伺服器的電腦名稱稱，符合在簽發給 VPN 伺服器的憑證中所定義的主體替代名稱。

若要確保遠端用戶端可以連線到您的 VPN 伺服器，您可以在外部 DNS 區域中建立 DNS A （主機）記錄。 A 記錄應該使用 VPN 伺服器的憑證主體替代名稱。

### <a name="to-add-a-host-a-or-aaaa-resource-record-to-a-zone"></a>將主機（A 或 AAAA）資源記錄新增至區域

1. 在 DNS 伺服器的伺服器管理員中，選取 [**工具**]，然後選取 [ **DNS**]。 [DNS 管理員] 隨即開啟。
2. 在 [DNS 管理員] 主控台樹中，選取您要管理的伺服器。
3. 在詳細資料窗格的 [**名稱**] 中，按兩下 [**正向對應區域**] 展開此視圖。
4. 在 [**正向對應區域**詳細資料] 中，以滑鼠右鍵按一下您要新增記錄的正向對應區域，然後選取 **[新增主機（a 或 AAAA）** ]。 [**新增主機**] 對話方塊隨即開啟。
5. 在 [**新主機**] 的 [**名稱**] 中，輸入 VPN 伺服器的憑證主體替代名稱。
6. 在 [IP 位址] 中，輸入 VPN 伺服器的 IP 位址。 您可以輸入 IP 版本4（IPv4）格式的位址，以新增主機（A）資源記錄，或使用 IP 版本6（IPv6）格式來新增主機（AAAA）資源記錄。
7. 如果您已針對某個範圍的 IP 位址（包括您所輸入的 IP 位址）建立反向對應區域，請選取 [**建立關聯的指標（PTR）記錄**] 核取方塊。  選取此選項會根據您在 [**名稱**] 和 [ **IP 位址**] 中輸入的資訊，在此主機的反向區域中建立額外的指標（PTR）資源記錄。
8. 選取 [**新增主機**]。

## <a name="configure-the-edge-firewall"></a>設定 Edge 防火牆

邊緣防火牆會將外部周邊網路與公用網際網路隔開。 如需這種分隔的視覺標記法，請參閱[ALWAYS ON VPN 技術總覽](../always-on-vpn-technology-overview.md)主題中的圖例。

您的邊緣防火牆必須允許並轉送特定埠到您的 VPN 伺服器。 如果您在邊緣防火牆上使用網路位址轉譯（NAT），您可能需要啟用使用者資料包協定（UDP）埠500和4500的埠轉送。 將這些埠轉送到指派給 VPN 伺服器之外部介面的 IP 位址。

如果您要路由傳送流量輸入，並在 VPN 伺服器後方執行 NAT，則必須開啟您的防火牆規則，以允許 UDP 埠500和4500輸入套用至 VPN 伺服器上公用介面的外部 IP 位址。

不論是哪一種情況，如果您的防火牆支援深層封包檢查，而且您無法建立用戶端連線，您應該嘗試放寬或停用 IKE 會話的深度封包檢查。

如需有關如何進行這些設定變更的資訊，請參閱您的防火牆檔。

## <a name="configure-the-internal-perimeter-network-firewall"></a>設定內部周邊網路防火牆

內部周邊網路防火牆會將組織/公司網路與內部周邊網路分開。 如需這種分隔的視覺標記法，請參閱[ALWAYS ON VPN 技術總覽](../always-on-vpn-technology-overview.md)主題中的圖例。

在此部署中，周邊網路上的遠端存取 VPN 伺服器會設定為 RADIUS 用戶端。  VPN 伺服器會將 RADIUS 流量傳送到公司網路上的 NPS，也會接收來自 NPS 的 RADIUS 流量。

設定防火牆以允許 RADIUS 流量雙向流動。

>[!NOTE]
>組織/公司網路上的 NPS 伺服器會做為 VPN 伺服器的 RADIUS 伺服器，也就是 RADIUS 用戶端。 如需 RADIUS 基礎結構的詳細資訊，請參閱[網路原則伺服器（NPS）](../../../../../networking/technologies/nps/nps-top.md)。

### <a name="radius-traffic-ports-on-the-vpn-server-and-nps-server"></a>VPN 伺服器和 NPS 伺服器上的 RADIUS 流量埠

根據預設，NPS 和 VPN 會在所有已安裝的網路介面卡上，接聽埠1812、1813、1645和1646上的 RADIUS 流量。 如果您在安裝 NPS 時啟用 [具有 Advanced Security 的 Windows 防火牆]，則會在 IPv6 和 IPv4 流量的安裝過程中自動建立這些埠的防火牆例外。

>[!IMPORTANT]
>如果您的網路存取伺服器設定為透過非預設的埠來傳送 RADIUS 流量，請移除在 NPS 安裝期間，在 [具有 Advanced Security 的 Windows 防火牆] 中建立的例外狀況，並為您要使用的埠建立例外。RADIUS 流量。

### <a name="use-the-same-radius-ports-for-the-internal-perimeter-network-firewall-configuration"></a>針對內部周邊網路防火牆設定使用相同的 RADIUS 埠

如果您在 VPN 伺服器和 NPS 伺服器上使用預設的 RADIUS 埠設定，請確定您在內部周邊網路防火牆上開啟下列埠：

- 埠 UDP1812、UDP1813、UDP1645 和 UDP1646

如果您不使用 NPS 部署中的預設 RADIUS 埠，您必須將防火牆設定為允許您所使用之埠上的 RADIUS 流量。 如需詳細資訊，請參閱[設定 RADIUS 流量的防火牆](../../../../../networking/technologies/nps/nps-firewalls-configure.md)。

## <a name="next-steps"></a>後續步驟

[步驟6。設定 Windows 10 用戶端 Always On VPN](vpn-deploy-client-vpn-connections.md)連線：在此步驟中，您會將 windows 10 用戶端電腦設定為使用 vpn 連線與該基礎結構進行通訊。 您可以使用數種技術來設定 Windows 10 VPN 用戶端，包括 Windows PowerShell、Microsoft Endpoint Configuration Manager 和 Intune。 這三個都需要 XML VPN 設定檔來設定適當的 VPN 設定。
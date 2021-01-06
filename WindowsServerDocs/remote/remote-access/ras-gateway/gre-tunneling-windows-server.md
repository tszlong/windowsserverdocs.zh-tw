---
title: Windows Server 2016 中的 GRE 通道
description: 您可以使用本主題來瞭解 Windows Server 2016 中的 RAS 閘道 (GRE) 通道功能的一般路由封裝更新。
manager: brianlic
ms.topic: article
ms.assetid: df2023bf-ba64-481e-b222-6f709edaa5c1
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: de33ba380da611dd4bece67da382ffd72d56e5eb
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947414"
---
# <a name="gre-tunneling-in-windows-server-2016"></a>Windows Server 2016 中的 GRE 通道

>適用於：Windows Server (半年度管道)、Windows Server 2016

Windows Server 2016 提供 RAS 閘道之一般路由封裝 \( GRE 通道 \) 功能的更新。

GRE 是輕量級的通道通訊協定，可透過網際網路通訊協定網路封裝各種不同的虛擬點對點連結內的網路層通訊協定。 Microsoft GRE 實行可以封裝 IPv4 和 IPv6。

GRE 通道在許多案例中很有用，因為：

-   它們是輕量且符合 RFC 2890 規範，使其可與各種廠商裝置互通

-   您可以使用邊界閘道協定 \( BGP \) 進行動態路由

-   您可以設定 GRE 多租使用者 RAS 閘道，以與軟體定義的網路 SDN 搭配使用 \(\)

-   您可以使用 System Center Virtual Machine Manager 來管理以 GRE 為 \- 基礎的 RAS 閘道

-   您最多可以在設定為 GRE RAS 閘道的6核心虛擬機器上達到 2.0 Gbps 輸送量

-   單一閘道支援多個連接模式

GRE 式通道可啟用租用戶虛擬網路與外部網路之間的連線。 由於 GRE 通訊協定是輕量的，且大部分的網路裝置都可以使用 GRE 的支援，因此它會成為不需要加密資料的通道的理想選擇。

站對站 (S2S) 通道中的 GRE 支援可解決使用多租使用者閘道在租使用者虛擬網路和租使用者外部網路之間轉送的問題，如本主題稍後所述。

GRE 通道功能的設計目的是要解決下列需求：

-   主機服務提供者必須能夠建立虛擬網路以供轉送，而不需要修改實體交換器設定。

-   主機服務提供者必須能夠將子網新增至其外部網路，而不需要修改其基礎結構內實體交換器的設定。
GRE 通道功能可啟用或增強幾個主要案例，以使用 Microsoft 技術來裝載服務提供者，以在其服務供應專案中執行軟體定義的網路功能。

以下是一些範例案例：

-   [從租使用者虛擬網路存取租使用者實體網路](#BKMK_Access)

-   [高速連線能力](#BKMK_Speed)

-   [與 VLAN 型隔離整合](#BKMK_Integration)

-   [存取共用資源](#BKMK_Shared)

-   [協力廠商裝置至租使用者的服務](#BKMK_thirdparty)

## <a name="key-scenarios"></a>主要案例

以下是 GRE 通道功能所解決的主要案例。

### <a name="access-from-tenant-virtual-networks-to-tenant-physical-networks"></a><a name="BKMK_Access"></a>從租使用者虛擬網路存取租使用者實體網路

此案例提供可調整的方式，可讓您從租使用者虛擬網路存取位於主機服務提供者部署上的租使用者實體網路。 在多租使用者閘道上建立 GRE 通道端點，在實體網路上的協力廠商裝置上建立另一個 GRE 通道端點。 第3層流量會在虛擬網路中的虛擬機器和實體網路上的協力廠商裝置之間路由傳送。

![連接主機服務提供者實體網路和租使用者虛擬網路的 GRE 通道](../../media/gre-tunneling-in-windows-server/GRE_.png)

### <a name="high-speed-connectivity"></a><a name="BKMK_Speed"></a>高速連線能力

此案例提供可調整的方式，提供從租使用者內部部署網路到其位於主機服務提供者網路中的虛擬網路的高速連線能力。 租使用者會透過多重通訊協定標籤切換 (MPLS) 來連線至服務提供者網路，在主機服務提供者的邊緣路由器與多租使用者閘道之間建立 GRE 通道至租使用者的虛擬網路。

![連接租使用者 enterprise MPLS 網路和租使用者虛擬網路的 GRE 通道](../../media/gre-tunneling-in-windows-server/GRE-.png)

### <a name="integration-with-vlan-based-isolation"></a><a name="BKMK_Integration"></a>與 VLAN 型隔離整合

此案例可讓您將 VLAN 型隔離與 Hyper-v 網路虛擬化整合。 主機服務提供者網路上的實體網路包含使用 VLAN 型隔離的負載平衡器。 多租使用者閘道會在實體網路上的負載平衡器與虛擬網路上的多租使用者閘道之間建立 GRE 通道。

可以在來源與目的地之間建立多個通道，而 GRE 金鑰則是用來區分通道。

![連接租使用者虛擬網路的多個 GRE 通道](../../media/gre-tunneling-in-windows-server/GRE-VLANIsolation.png)

### <a name="access-shared-resources"></a><a name="BKMK_Shared"></a>存取共用資源

此案例可讓您存取位於主機服務提供者網路上的實體網路上的共用資源。

您的共用服務可能是位於主機服務提供者網路中，您想要與多個租使用者虛擬網路共用的實體網路上的伺服器。

具有非重迭子網的租使用者網路會透過 GRE 通道存取通用網路。 單一租使用者閘道會在 GRE 通道之間路由傳送，因此會將封包路由傳送到適當的租使用者網路。

在此案例中，單一租使用者閘道可由協力廠商硬體設備取代。

![使用多個通道連接多個虛擬網路的單一租使用者閘道](../../media/gre-tunneling-in-windows-server/GRE-SharedResource.png)

### <a name="services-of-third-party-devices-to-tenants"></a><a name="BKMK_thirdparty"></a>協力廠商裝置至租使用者的服務

此案例可用來將協力廠商裝置 (例如硬體負載平衡器) 至租使用者虛擬網路流量流程。 例如，來自企業網站的流量會通過 S2S 通道傳送至多租使用者閘道。 流量會透過 GRE 通道路由傳送至負載平衡器。 負載平衡器會將流量路由至企業虛擬網路上的多部虛擬機器。 在虛擬網路中有可能重迭 IP 位址的另一個租使用者也會發生相同的情況。 網路流量會在負載平衡器上使用 Vlan 隔離，而且適用于所有支援 Vlan 的第3層裝置。

![將虛擬網路連接到協力廠商裝置的多個 GRE 通道](../../media/gre-tunneling-in-windows-server/GREThirdParty.png)

## <a name="configuration-and-deployment"></a>設定和部署

GRE 通道會公開為 S2S 介面內的其他通訊協定。 它的執行方式類似于下列網路功能 Blog 所述的 IPSec S2S 通道： [多租使用者站對站 (S2S) VPN 閘道與 Windows Server 2012 R2](https://techcommunity.microsoft.com/t5/networking-blog/bg-p/NetworkingBlog)

請參閱下列主題，以取得部署閘道的範例，包括 GRE 通道閘道：

[使用指令碼部署軟體定義網路的基礎結構](../../../networking/sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)

## <a name="more-information"></a>詳細資訊

如需部署 S2S 閘道的詳細資訊，請參閱下列主題：

-   [RAS 閘道](RAS-Gateway.md)

-   [邊界閘道協定 &#40;BGP&#41;](../bgp/Border-Gateway-Protocol-BGP.md)

-   [新增功能！Windows Server 2012 R2 RAS 多租使用者閘道部署指南](https://techcommunity.microsoft.com/t5/networking-blog/bg-p/NetworkingBlog)

-   [使用 RAS 多租使用者閘道部署邊界閘道協定 (BGP) ](https://techcommunity.microsoft.com/t5/networking-blog/bg-p/NetworkingBlog)


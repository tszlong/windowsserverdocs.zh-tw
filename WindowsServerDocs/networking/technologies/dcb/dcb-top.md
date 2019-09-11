---
title: 資料中心橋接 (DCB)
description: 您可以使用本主題，取得有關 Windows Server 2016 中資料中心橋接的簡介資訊。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6446324e23b9f1c87cb28bd76feb6cb493db8253
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869758"
---
# <a name="data-center-bridging-dcb"></a>資料中心橋\(接 DCB\)

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題，取得有關資料中心橋接\(DCB\)的簡介資訊。

DCB 是一組電氣和電子網工程師\( \) ，可啟用資料中心內的聚合式網狀架構，其中儲存體、資料網路、叢集進程間\-通訊\( IPC\)和管理流量全都會共用相同的 Ethernet 網路基礎結構。

>[!NOTE]
>除了本主題之外，還有下列 DCB 檔可供使用
>
>- [在 Windows Server 2016 或 Windows 10 中安裝 DCB](dcb-install.md)
>- [管理資料中心橋接（DCB）](dcb-manage.md)

DCB 提供以\-硬體為主的頻寬配置給特定類型的網路流量，並使用以優先順序\-為基礎的流量控制來增強乙太網路傳輸可靠性。

如果\-流量略過作業系統並卸載至交集式網路介面卡（可能支援網際網路小型電腦系統介面\(iSCSI\)），則以硬體為基礎的頻寬配置是不可或缺的。透過乙太網路的\(遠端\)直接記憶體存取 RDMA，或透過乙太\(網路\)的光纖通道 FCoE。

如果\-上層通訊協定（如光纖通道）採用不失真的基礎傳輸，則以優先順序為基礎的流量控制是不可或缺的。

## <a name="dcb-protocols-and-management-options"></a>DCB 通訊協定和管理選項

DCB 包含下列一組通訊協定。 

- 增強型傳輸\(服務\) ETS – IEEE 802.1 qaz，以 802.1 p 和 802.1 q 標準為基礎
- 優先順序流程式控制制\(PFS\)，IEEE 802.1 qbb 
- DCB Exchange Protocol \(DCBX\)，IEEE 802.1 ab，已在 802.1 qaz standard 中擴充。

DCBX 通訊協定可讓您在交換器上設定 DCB，然後再自動設定終端裝置，例如執行 Windows Server 2016 的電腦。

如果您想要從交換器管理 DCB，則不需要在 Windows Server 2016 中安裝 DCB 做為功能，不過這種方法包含一些限制。

由於 DCBX 只能通知主機網路介面卡 ETS 類別大小和 PFC 啟用，因此 Windows Server 主機通常會要求安裝 DCB，讓流量對應到 ETS 類別。

Windows 應用程式通常不是用來參與 DCBX 交換。 因此，主機必須與網路交換器分開設定，但使用相同的設定。

如果您選擇從交換器管理 DCB，您仍然可以使用 Windows PowerShell 命令來查看 Windows Server 2016 中的設定。

##  <a name="important-dcb-functionality"></a>重要的 DCB 功能

下面的清單摘要說明 DCB 提供的功能。

1. 提供 DCB\-功能的網路介面卡與支援 DCB\-的交換器之間的互通性。

2. 在網路介面卡上開啟以優先順序\-為基礎的流量控制，在執行 Windows Server 2016 的電腦與它的鄰近交換器之間，提供不失真的 Ethernet 傳輸。

3. 可讓您依百分比將頻寬配置給流量\(控制\) TC，其中 tc 可能包含一或多個以 802.1 p 流量類別\(優先順序\)區別的流量類別'.

4. 能讓伺服器系統管理員或網路系統管理員將應用程式指派給特定流量類別，或是根據這個應用程式使用的已知通訊協定、已知 TCP/UDP 連接埠或 NetworkDirect 連接埠來設定優先順序。

5. 透過 windows Server 2016 Windows Management Instrumentation \(WMI\)和 windows PowerShell 提供 DCB 管理。 如需詳細資訊，請參閱本主題稍後的[適用于 DCB 的 Windows PowerShell 命令](#bkmk_wps)一節，以及下列主題。
    - [系統提供的 DCB 元件](https://msdn.microsoft.com/windows/hardware/drivers/network/system-provided-dcb-components)
    - [資料中心橋接的 NDIS QoS 需求](https://msdn.microsoft.com/windows/hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. 透過 Windows Server 2016 群組原則提供 DCB 管理。

7. 支援 Windows Server 2016 服務\(品質 QoS\)解決方案的共存。

>[!NOTE]
>使用 rdma 的聚合式 Ethernet \(RoCE\)版本的任何 rdma 之前，您必須先啟用 DCB。 雖然網際網路範圍 rdma 通訊協定\(iWARP\)網路不需要，但測試已判定所有以乙太\-網路為基礎的 rdma 技術，都能更適合 DCB。 因此，您應該考慮使用 DCB 來進行 iWARP RDMA 部署。 如需詳細資訊，請參閱[遠端直接記憶體存取（RDMA）和交換器內嵌小組（SET）](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

##  <a name="practical-applications-of-dcb"></a>DCB 的實際應用

許多組織都有適用于\(儲存體服務的大型\(光纖\)通道 FC\)存放區域網路 SAN 安裝。 FC SAN 的伺服器需要特殊的網路介面卡，而且網路中需要 FC 交換器。 這些組織通常也會使用乙太網路介面卡和交換器。

在大部分情況下，部署 FC 硬體比 Ethernet 硬體高得多，因此會導致大量的資本支出。 此外，個別 Ethernet 和 FC SAN 網路介面卡和交換器硬體的需求需要資料中心內的額外空間、電力和冷卻能力，因而導致額外的營運支出。

從成本的觀點來看，許多組織將其 FC 技術與以乙太\-網路為基礎的硬體解決方案合併，以提供儲存體和資料網路服務的優點。

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>針對儲存體和資料網路\-使用以 Ethernet 為基礎的聚合式網狀架構 DCB

對於已經有大型 FC SAN，但想要從 FC 技術的額外投資中遷移的組織，DCB 可讓您針對儲存體和資料網路功能建立以乙太網路為基礎的聚合式網狀架構。 DCB 聚合式網狀架構可以降低未來總擁有權\(TCO\)的成本，並簡化管理工作。

對於已經採用或打算採用 iscsi 作為其儲存體解決方案的主控者，DCB 可以為 iscsi 流量提供硬體\-輔助頻寬保留，以確保效能隔離。 與其他專屬解決方案不同的是，DCB\-是以標準為基礎，因此相當容易在不同的網路環境中部署和管理。

以 Windows Server 2016\-為基礎的 DCB 執行，可減輕多個原始設備\)製造商\(提供的聚合式網狀架構解決方案時，所發生的許多問題。

由多個 Oem 提供的專屬解決方案，可能無法彼此交互操作、可能很容易管理，而且通常會有不同的軟體維護排程。 

相反地，Windows Server 2016 DCB 是以\-標準為基礎，您可以在異類網路中部署和管理 DCB。

## <a name="bkmk_wps"></a>適用于 DCB 的 Windows PowerShell 命令

Windows Server 2016 和 Windows Server 2012 R2 都有 DCB 的 Windows PowerShell 命令。 您可以在 Windows Server 2016 中，使用 Windows Server 2012 R2 的所有命令。

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>適用于 DCB 的 windows Server 2016 Windows PowerShell 命令

下列適用于 windows Server 2016 的主題提供 windows PowerShell Cmdlet 的說明和語法，適用于\(所有\)的資料中心\(橋\)接 DCB 服務 QoS\-特定 Cmdlet 的品質。 Cmdlet 清單以 Cmdlet 開頭動詞的字母順序排列。

- [DcbQoS 模組](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>適用于 DCB 的 windows Server 2012 R2 Windows PowerShell 命令

下列適用于 windows Server 2012 R2 的主題提供 windows PowerShell Cmdlet 的說明和語法，適用于\(所有\)資料中心橋\(接\)DCB 服務 QoS\-特定 Cmdlet 的品質。 Cmdlet 清單以 Cmdlet 開頭動詞的字母順序排列。

- [Windows PowerShell 中的資料中心橋接（DCB）服務品質（QoS） Cmdlet](https://technet.microsoft.com/library/hh967440.aspx)

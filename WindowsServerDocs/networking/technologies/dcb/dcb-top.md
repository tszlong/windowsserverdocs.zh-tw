---
title: 資料中心橋接 (DCB)
description: 您可以使用本主題取得有關 Windows Server 2016 中的資料中心橋接的簡介資訊。
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: eab47e24ad8c2bd5436b3516babee9237c3c80f3
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865567"
---
# <a name="data-center-bridging-dcb"></a>\(資料中心橋接 (DCB)\)

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來取得有關資料中心橋接 DCB 的簡介資訊 \( \) 。

DCB 是一組電氣和電子技術工程師 \( IEEE \) 標準，可在資料中心啟用交集網狀架構，其中的儲存體、資料網路功能、跨 \- 進程通訊 \( IPC \) 的叢集，以及管理流量全都共用相同的乙太網路基礎結構。

>[!NOTE]
>除了本主題之外，還提供下列 DCB 檔
>
>- [在 Windows Server 2016 或 Windows 10 中安裝 DCB](dcb-install.md)
>- [管理資料中心橋接 (DCB) ](dcb-manage.md)

DCB \- 針對特定類型的網路流量提供以硬體為基礎的頻寬配置，並使用以優先順序為基礎的流量控制來增強乙太網路傳輸可靠性 \- 。

\-如果流量略過作業系統並卸載至交集網路介面卡（可能支援網際網路小型電腦系統介面 \( iSCSI \) 、透過乙太網路的遠端直接記憶體存取 RDMA，或透過乙太網路 \( \) 的光纖通道 FCoE \( \) ），則以硬體為基礎的頻寬配置是不可或缺的。

\-如果上層通訊協定（例如光纖通道）採用不失真的基礎傳輸，則以優先順序為基礎的流量控制是不可或缺的。

## <a name="dcb-protocols-and-management-options"></a>DCB 通訊協定和管理選項

DCB 包含下列一組通訊協定。

- 增強型傳輸服務 \( ETS \) – IEEE 802.1 qaz，其建基於 802.1 p 和 802.1 q 標準
- 優先權流程式控制制 \( PFC \) ，IEEE 802.1 qbb
- DCB Exchange Protocol \( DCBX \) ，IEEE 802.1 ab，如 802.1 qaz standard 中的延伸。

DCBX 通訊協定可讓您設定交換器上的 DCB，然後自動設定終端設備，例如執行 Windows Server 2016 的電腦。

如果您想要從交換器管理 DCB，就不需要在 Windows Server 2016 中安裝 DCB 功能，不過這種方法包含一些限制。

因為 DCBX 只會通知主機網路介面卡 ETS 類別大小和 PFC 啟用，所以 Windows Server 主機通常需要安裝 DCB，以便將流量對應到 ETS 類別。

Windows 應用程式通常不會用來參與 DCBX 交換。 因此，主機必須與網路交換器分開設定，但使用相同的設定。

如果您選擇從交換器管理 DCB，仍然可以使用 Windows PowerShell 命令來查看 Windows Server 2016 中的設定。

##  <a name="important-dcb-functionality"></a>重要的 DCB 功能

下面的清單摘要說明 DCB 提供的功能。

1. 提供 DCB 支援的 \- 網路介面卡與 DCB \- 功能的交換器之間的互通性。

2. 藉由開啟網路介面卡上以優先權為基礎的流量控制，提供執行 Windows Server 2016 的電腦與其鄰近交換器之間的不失真乙太 \- 網路傳輸。

3. 提供依百分比將頻寬分配給流量控制 tc 的能力 \( \) ，其中 tc 可能包含一或多個由 802.1 p 流量類別優先順序指標區分的流量類別 \( \) 。

4. 能讓伺服器系統管理員或網路系統管理員將應用程式指派給特定流量類別，或是根據這個應用程式使用的已知通訊協定、已知 TCP/UDP 連接埠或 NetworkDirect 連接埠來設定優先順序。

5. 透過 Windows Server 2016 Windows Management Instrumentation \( WMI 和 Windows PowerShell 提供 DCB 管理 \) 。 如需詳細資訊，請參閱本主題稍後的 [WINDOWS POWERSHELL DCB 命令](#bkmk_wps) 章節，以及下列主題。
    - [系統提供的 DCB 元件](/windows-hardware/drivers/network/system-provided-dcb-components)
    - [資料中心橋接的 NDIS QoS 需求](/windows-hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. 透過 Windows Server 2016 群組原則提供 DCB 管理。

7. 支援 Windows Server 2016 服務品質 QoS 解決方案的共存 \( \) 。

>[!NOTE]
>在透過交集 Ethernet \( RoCE 版本的 rdma 使用任何 rdma 之前 \) ，您必須先啟用 DCB。 雖然網際網路廣域網路區域 RDMA 通訊協定 iWARP 網路並不需要 \( \) ，但測試已判定所有 \- 以乙太網路為基礎的 RDMA 技術在 DCB 方面都能更有效地運作。 基於這個原因，您應該考慮使用 DCB 來 iWARP RDMA 部署。 如需詳細資訊，請參閱 [ (RDMA 的遠端直接記憶體存取) 和切換內嵌小組 (設定) ](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

##  <a name="practical-applications-of-dcb"></a>DCB 的實際應用

許多組織都有 \( \) \( \) 適用于儲存體服務的大型光纖通道 FC 儲存區域網路 SAN 安裝。 FC SAN 的伺服器需要特殊的網路介面卡，而且網路中需要 FC 交換器。 這些組織通常也會使用 Ethernet 網路介面卡和交換器。

在大多數情況下，隨著乙太網路硬體的部署，FC 硬體的部署成本會大幅增加，而導致大量的資本支出。 此外，不同的乙太網路和 FC SAN 網路介面卡和交換器硬體的需求，在資料中心內需要額外的空間、電源和冷卻容量，以產生額外的持續營運支出。

從成本的觀點來看，許多組織都可以使用其以乙太網路為基礎的硬體解決方案來合併 FC 技術， \- 以提供儲存體和資料網路服務。

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>針對 \- 儲存和資料網路使用以乙太網路為基礎的融合式網狀架構 DCB

對於已經有大型 FC SAN 但想要遷移到 FC 技術的額外投資的組織，DCB 可讓您建立以乙太網路為基礎的融合式網狀架構，以進行儲存體和資料網路。 DCB 的融合式網狀架構可減少擁有權 TCO 的未來總成本 \( \) ，並簡化管理。

針對已採用或打算採用 iSCSI 作為其儲存體解決方案的主控者，DCB 可以為 \- iscsi 流量提供硬體輔助的頻寬保留，以確保效能隔離。 與其他專屬解決方案不同的是，DCB 是以標準為 \- 基礎，因此相當容易在不同的網路環境中部署和管理。

以 Windows Server 2016 為 \- 基礎的 DCB，可減輕多個原始設備製造商 oem 提供交集網狀架構解決方案時可能發生的許多 \( 問題 \) 。

由多個 Oem 提供的專屬解決方案可能無法彼此交互操作、可能很難管理，而且通常會有不同的軟體維護排程。

相較之下，Windows Server 2016 DCB 是 \- 以標準為基礎，而且您可以在異類網路中部署和管理 DCB。

## <a name="windows-powershell-commands-for-dcb"></a><a name="bkmk_wps"></a>適用于 DCB 的 Windows PowerShell 命令

Windows Server 2016 和 Windows Server 2012 R2 都有 DCB Windows PowerShell 命令。 您可以在 Windows Server 2016 中使用 Windows Server 2012 R2 的所有命令。

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>適用于 DCB 的 Windows Server 2016 Windows PowerShell 命令

適用于 Windows Server 2016 的下列主題提供 Windows PowerShell 的 Cmdlet 說明和語法，適用于所有資料中心橋接 \( DCB \) 服務 \( QoS \) \- 特定 Cmdlet 的品質。 Cmdlet 清單以 Cmdlet 開頭動詞的字母順序排列。

- [DcbQoS 模組](/powershell/module/dcbqos/)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>適用于 DCB 的 Windows Server 2012 R2 Windows PowerShell 命令

下列適用于 Windows Server 2012 R2 的主題提供 Windows PowerShell 的 Cmdlet 說明和語法，適用于所有資料中心橋接 \( DCB \) 服務 \( QoS \) \- 特定 Cmdlet 的品質。 Cmdlet 清單以 Cmdlet 開頭動詞的字母順序排列。

- [Windows PowerShell 中的資料中心橋接 (DCB) 服務品質 (QoS) Cmdlet](/powershell/module/dcbqos/)

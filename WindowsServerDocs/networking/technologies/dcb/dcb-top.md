---
title: 資料中心橋接 (DCB)
description: 您可以使用本主題適用於簡介資料中心橋接 Windows Server 2016 中的資訊。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3d465e855adc387d7136919ac11fbab56c792c34
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="data-center-bridging-dcb"></a>資料中心橋接 \(DCB\)

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題的資料中心橋接 \(DCB\) 簡介資訊。

DCB 是一套協會和電子工程師 \(IEEE\) 標準，可讓資料中心，儲存空間、資料網路、叢集 Inter\ 處理序通訊 \(IPC\) 和管理流量所有分享相同乙太網路基礎結構匯集的架構。

>[!NOTE]
>本主題中，除了下列 DCB 文件已可使用
>
>- [Windows Server 2016 或 Windows 10 中安裝 DCB](dcb-install.md)
>- [管理資料中心橋接 (DCB)](dcb-manage.md)

DCB 提供 hardware\ 為基礎的頻寬特定類型的網路流量配置，並美化乙太網路傳輸使用 priority\ 型流程控制項的可靠性。

如果流量略過作業系統，便會至聚合型的網路介面卡，可能會支援網際網路小型的電腦系統介面 \(iSCSI\)、遠端直接記憶體存取 \(RDMA\) 透過乙太網路或光纖通道上乙太網路 \(FCoE\) 卸載，Hardware\ 型頻寬配置很重要。

Priority\ 型流程控制項是不可或缺上層通訊協定，光纖的通道，例如假設無失真基礎傳輸。

## <a name="dcb-protocols-and-management-options"></a>DCB 通訊協定及管理選項

DCB 包含下列設定通訊協定。 

- 美化的傳輸服務 \(ETS\) – IEEE 802.1Qaz，此組建上 802.1 P 802.1Q 標準
- 優先順序流程控制 \(PFS\)，IEEE 802.1Qbb 
- IEEE 802.1AB，以在標準 802.1Qaz 延伸 DCB 交換通訊協定 \(DCBX\)。

DCBX 通訊協定可讓您設定 DCB 切換，然後就可以自動設定結束裝置，例如執行 Windows Server 2016 的電腦上。

如果您想要切換的管理 DCB，您不需要安裝 DCB 與 Windows Server 2016 中的功能但這種方式包含一些限制。

因為 DCBX 僅能通知 ETS 課程大小和 PFC 提供的主機網路介面卡，不過，Windows Server 主機通常需要 DCB，以便流量對應至 ETS 類別安裝。

Windows 應用程式通常不專為參與 DCBX 交換。 因此，從網路參數，但有相同的設定，則必須分開設定主機。

如果您選擇管理 DCB 從切換，您仍然可以使用 Windows PowerShell 命令 Windows Server 2016 中檢視設定。

##  <a name="important-dcb-functionality"></a>重要 DCB 功能

以下是清單摘要 DCB 提供的功能。

1. 提供交互操作 DCB\ 能力網路介面卡之間切換 DCB\ 處理能力。

2. 提供無失真乙太網路傳輸之間執行 Windows Server 2016 和其鄰居切換經由 priority\ 型流程控制項的網路介面卡上的電腦。

3. 提供交通控制 \(TC\) 頻寬配置百分比，其中一或多個種流量之 802.1 p 流量課程 \(priority\) 指示器來區分可能包含 TC 的能力。

4. 可讓伺服器管理員或網路系統管理員指派應用程式特定的流量課程或優先順序根據已知通訊協定、已知 TCP 日 UDP 連接埠或該應用程式所使用的 NetworkDirect 連接埠。

5. 提供 Windows Server 2016 Windows 管理檢測 \(WMI\) 及 Windows PowerShell 透過 DCB 管理。 如需詳細資訊，請查看區段[Windows PowerShell 命令 DCB](#bkmk_wps)稍後本主題中，除了下列主題。
    - [系統提供 DCB 元件](https://msdn.microsoft.com/windows/hardware/drivers/network/system-provided-dcb-components)
    - [適用於資料中心橋接需求 NDIS 品質](https://msdn.microsoft.com/windows/hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. 提供 DCB 管理透過 Windows Server 2016 群組原則。

7. 支援 Windows Server 2016 服務品質 \(QoS\) 方案的並存。

>[!NOTE]
>您必須先使用任何 RDMA 透過 RDMA 匯集乙太網路 \(RoCE\) 版本，可讓 DCB。 雖然不所需的網際網路寬區域 RDMA 通訊協定 \(iWARP\) 網路，測試發現所有 Ethernet\ 型 RDMA 技術變得更好都搭配 DCB。 因此，您應該使用 DCB iWARP RDMA 部署。 如需詳細資訊，請查看[遠端直接記憶體存取 (RDMA) 和切換 Embedded 小組（設定）](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

##  <a name="practical-applications-of-dcb"></a>實用的 DCB 的應用程式

許多公司擁有大型光纖通道 \(FC\) 儲存區域網路 \(SAN\) 安裝的儲存空間服務。 FC 舊需要伺服器 FC 參數，在網路上的特殊的網路介面卡。 這些組織通常也會使用乙太網路卡和切換。

在大部分案例中，FC 硬體會大幅價格部署比乙太網路硬體，會導致大大寫費用。 此外，另一個乙太網路與 FC 舊網路介面卡和切換硬體需求需要更多空間，電源，和冷卻 datacenter，會導致其他、持續運作的費用。

成本觀點，則也有許多組織合併 FC 技術儲存和資料網路服務提供其 Ethernet\ 型硬體方案的好處。

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>使用 DCB Ethernet\ 型聚合型 fabric，儲存的資料網路

為組織已經有大量的 FC 舊，但想要從其他投資 FC 技術移轉 DCB 可讓您建置根據乙太網路匯集 fabric 儲存和資料網路。 DCB 匯集 fabric 可以減少取得未來成本 \(TCO\) 簡化管理。

誰有採用，或人員計劃收養主機創造，做為其儲存方案，DCB iSCSI 可以提供 iSCSI 流量確保效能隔離 hardware\ 協助頻寬保留。 並與其他專屬方案，DCB 是 standards\--，因此較為輕鬆地部署與管理異質性網路環境中。

Windows Server 2016\ 型 DCB 實作可以時，可能發生匯集 fabric 方案所提供的多個原始設備製造商 \(OEMs\) 的問題。

提供多個的 Oem 專用方案實作可能無法與其他互相作用，可能會很困難管理及通常會有不同的軟體維護排程。 

相較之下，Windows Server 2016 DCB standards\ 為基礎，且部署及管理 DCB 異質性網路。

## <a name="bkmk_wps"></a>Windows PowerShell 命令 DCB

有適用於 Windows Server 2016 和 Windows Server 2012 R2 DCB Windows PowerShell 命令。 您可以使用的命令所有的 Windows Server 2016 中的 Windows Server 2012 R2。

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Windows Server 2016 Windows PowerShell 命令 DCB

下列主題適用於 Windows Server 2016 的所有的資料中心橋接 \(DCB\) 提供 Windows PowerShell cmdlet 描述和語法服務品質 \ (QoS\) \-specific cmdlet。 它會列出 cmdlet 依字母順序根據動詞 cmdlet 的開頭。

- [DcbQoS 模組](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Windows Server 2012 R2 Windows PowerShell 命令 DCB

下列主題適用於 Windows Server 2012 R2 的所有的資料中心橋接 \(DCB\) 提供 Windows PowerShell cmdlet 描述和語法服務品質 \ (QoS\) \-specific cmdlet。 它會列出 cmdlet 依字母順序根據動詞 cmdlet 的開頭。

- [資料中心橋接 (DCB) 品質的 Windows PowerShell 中的服務 (QoS) Cmdlet](https://technet.microsoft.com/library/hh967440.aspx)

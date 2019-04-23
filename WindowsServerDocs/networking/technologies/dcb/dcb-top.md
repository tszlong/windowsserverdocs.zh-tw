---
title: 資料中心橋接 (DCB)
description: 您可以使用本主題的資料中心橋接在 Windows Server 2016 中的簡介資訊。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cb488192a873743db27d9c1d09c5912bc810bb8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834179"
---
# <a name="data-center-bridging-dcb"></a>資料中心橋接\(DCB\)

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題的資料中心橋接的簡介資訊\(DCB\)。

DCB 是一套美國電機暨電子工程師\(IEEE\)啟用聚合式資料中心，其中的儲存體、 資料網路、 叢集間的網狀架構的標準\-處理序通訊\(IPC\)，以及管理流量的所有共用相同的乙太網路基礎結構。

>[!NOTE]
>除了本主題中，會提供下列 DCB 文件
>
>- [在 Windows Server 2016 或 Windows 10 安裝 DCB](dcb-install.md)
>- [管理資料中心橋接 (DCB)](dcb-manage.md)

DCB 提供硬體\-架構特定類型的網路流量的頻寬配置，並增強乙太網路傳輸可靠性與優先順序使用\-型流量控制。

硬體\-根據的頻寬配置就很重要，如果流量會略過作業系統而且卸載到聚合式的網路介面卡，它可以支援 Internet Small Computer System Interface \(iSCSI\)，遠端直接記憶體存取\(RDMA\)透過乙太網路或透過乙太網路的光纖通道\(FCoE\)。

優先順序\-型的流量控制是不可或缺的較高層通訊協定，如光纖通道會假設不失真的基礎傳輸。

## <a name="dcb-protocols-and-management-options"></a>DCB 的通訊協定和管理選項

DCB 是由下列通訊協定的集合所組成。 

- 增強的傳輸服務\(ETS\) – IEEE 802.1Qaz 和為基礎的 802.1p 802.1Q 標準
- 優先順序流量控制\(PFS\)，IEEE 802.1Qbb 
- DCB 交換通訊協定\(DCBX\)，IEEE 802.1AB，做為標準 802.1Qaz 中擴充。

DCBX 通訊協定可讓您設定的參數，可以再自動設定裝置，例如執行 Windows Server 2016 的電腦上的 DCB。

如果您想要從交換器管理 DCB，您不需要安裝 DCB 做為功能在 Windows Server 2016 中，不過這種方法還包含一些限制。

因為 DCBX 只會通知主機網路介面卡，ETS 類別大小和 PFC 啟用的不過，Windows Server 主機通常需要 DCB 已安裝，以便將流量對應至 ETS 類別。

若要參與 DCBX 交換通常並非設計 Windows 應用程式。 因為這個緣故，來自網路交換器，但具有相同的設定，則必須個別設定主應用程式。

如果您選擇來管理 DCB 的切換，您仍然可以使用 Windows PowerShell 命令來檢視 Windows Server 2016 中設定。

##  <a name="important-dcb-functionality"></a>重要的 DCB 功能

下面的清單摘要說明 DCB 提供的功能。

1. 提供 DCB 之間的互通性\-支援網路介面卡和 DCB\-交換器。

2. 提供不會損耗的乙太網路傳輸的電腦上開啟以優先順序執行 Windows Server 2016 和及其週邊交換器之間\-型流量控制的網路介面卡。

3. 讓您能夠將頻寬配置給流量控制\(TC\)百分比，其中 TC 可能包含一或多個流量類別 802.1p 流量類別來區分\(優先順序\)指標。

4. 能讓伺服器系統管理員或網路系統管理員將應用程式指派給特定流量類別，或是根據這個應用程式使用的已知通訊協定、已知 TCP/UDP 連接埠或 NetworkDirect 連接埠來設定優先順序。

5. 提供透過 Windows Server 2016 的 Windows Management Instrumentation 來管理 DCB \(WMI\)和 Windows PowerShell。 如需詳細資訊，請參閱[DCB 的 Windows PowerShell 命令](#bkmk_wps)稍後本主題中，除了下列主題。
    - [系統提供 DCB 的元件](https://msdn.microsoft.com/windows/hardware/drivers/network/system-provided-dcb-components)
    - [資料中心橋接的 NDIS QoS 需求](https://msdn.microsoft.com/windows/hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. 提供透過 Windows Server 2016 的 群組原則來管理 DCB。

7. 支援 Windows Server 2016 服務品質的共存\(QoS\)解決方案。

>[!NOTE]
>之前使用透過聚合式乙太網路的任何 RDMA \(RoCE\)版本的 RDMA，您必須啟用 DCB。 雖然不需要網際網路寬區域 RDMA 通訊協定\(iWARP\)網路，測試已判斷出所有的乙太網路\-基礎 DCB 以達到更好的 RDMA 技術工作。 基於這個原因，您應該考慮使用 DCB iWARP RDMA 部署。 如需詳細資訊，請參閱 <<c0> [ 遠端直接記憶體存取 (RDMA) 和 Switch Embedded Teaming (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

##  <a name="practical-applications-of-dcb"></a>DCB 的實際應用

許多組織都有大型的光纖通道\(FC\)存放區域網路\(SAN\)儲存體服務的安裝。 FC SAN 的伺服器需要特殊的網路介面卡，而且網路中需要 FC 交換器。 這些組織通常也會使用乙太網路介面卡和交換器。

在大部分情況下，FC 硬體的費用會比硬體乙太網路硬體，這會產生大量資本部署。 此外，個別乙太網路及 FC SAN 網路介面卡和交換器硬體的需求所需要的額外空間、 電源及冷卻性能，在資料中心，會導致其他的持續營運支出。

成本的觀點而言，很有幫助，對許多組織來說，若要合併它們的 FC 技術，其乙太網路\-基礎硬體解決方案，以提供儲存體和網路服務的資料。

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>使用 DCB 的乙太網路\-聚合式結構的儲存體及資料網路服務

針對已經擁有大型 FC SAN 但想要將從額外的投資 FC 技術轉移的組織，DCB 可讓您建置基礎的乙太網路聚合式儲存體及資料網路服務的網狀架構。 DCB 聚合式網狀架構可以降低未來的擁有權總成本\(TCO\)和簡化管理。

主機服務提供者已經採用，或使用者採用計劃，iSCSI 作為儲存體解決方案，DCB 可以提供硬體\-協助為以確保效能隔離 iSCSI 流量保留頻寬。 不同於其他專屬的解決方案，DCB 是以標準和\-根據-，因此相對較容易在異質網路環境中部署和管理。

Windows Server 2016\-DCB 的基礎的實作會減輕問題時可能發生的聚合式結構解決方案由多個原始設備製造商提供的許多\(Oem\)。

實作多個 Oem 所提供的專屬解決方案彼此之間無法互通，可能會難以管理，而且通常會有不同的軟體維護排程。 

相反地，Windows Server 2016 DCB 是以標準\-為基礎，且您可以部署並在異質網路中管理 DCB。

## <a name="bkmk_wps"></a>DCB 的 Windows PowerShell 命令

有 Windows Server 2016 和 Windows Server 2012 R2 的 DCB Windows PowerShell 命令。 您可以使用的所有命令的 Windows Server 2016 中的 Windows Server 2012 R2。

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Windows Server 2016 Windows PowerShell 命令，DCB

Windows Server 2016 的下列主題提供 Windows PowerShell cmdlet 說明和語法的所有資料中心橋接\(DCB\)服務品質\(QoS\)\-特定 cmdlet。 Cmdlet 清單以 Cmdlet 開頭動詞的字母順序排列。

- [DcbQoS Module](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Windows Server 2012 R2 Windows PowerShell 命令，DCB

Windows Server 2012 R2 的下列主題提供 Windows PowerShell cmdlet 說明和語法的所有資料中心橋接\(DCB\)服務品質\(QoS\)\-特定 cmdlet。 Cmdlet 清單以 Cmdlet 開頭動詞的字母順序排列。

- [資料中心橋接 (DCB) 服務品質 (QoS) Cmdlet 在 Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)

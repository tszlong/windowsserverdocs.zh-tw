---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: 簡化 SMB 多重通道和多個 NIC 的叢集網路
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 7fad43cb5f3de5c10ed815fa802b6168c15850d1
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990754"
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>簡化 SMB 多重通道和多個 NIC 的叢集網路

> 適用於：Windows Server 2019、Windows Server 2016

簡化的 SMB 多重通道和多個<abbr title="網路介面卡">NIC</abbr> 叢集網路是一項功能，可讓您在相同的叢集網路子網上使用多個 Nic，並自動啟用 SMB 多重通道。

簡化的 SMB 多重通道和多個 NIC 叢集網路提供下列優點：
- 容錯移轉叢集會自動辨識使用相同交換器/相同子網之節點上的所有 Nic-不需要額外的設定。
- SMB 多重通道會自動啟用。
- 只有 IPv6 連結本機 (fe80) IP 位址資源的網路，才會在僅限叢集 (私人) 網路上辨識。
- 預設會在每個叢集存取點上設定單一 IP 位址資源 (端點) 網路名稱 (NN) 。
- 在相同子網上找到多個 Nic 時，叢集驗證就不會再發出警告訊息。

## <a name="requirements"></a>需求
-   每個伺服器使用相同的交換器/子網的多個 Nic。

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>如何利用多個 NIC 的叢集網路和簡化的 SMB 多重通道
本節說明如何利用新的多個 NIC 叢集網路和簡化的 SMB 多重通道功能。

### <a name="use-at-least-two-networks-for-failover-clustering"></a>使用至少兩個網路來進行容錯移轉叢集
雖然很罕見，但網路交換器可能會失敗，但最佳作法是使用至少兩個網路來進行容錯移轉叢集。 所有找到的網路都會用於叢集的心跳。 請避免針對您的容錯移轉叢集使用單一網路，以避免發生單一失敗點。 在理想的情況下，叢集中的節點之間應該有多個實體通訊路徑，而不會有單一失敗點。

![兩個容錯移轉叢集網路的圖例 ](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig1.png)
 **圖1：使用至少兩個網路來進行容錯移轉**叢集

### <a name="use-multiple-nics-across-clusters"></a>跨叢集使用多個 Nic

當跨叢集使用多個 Nic 時，可以達到簡化 SMB 多重通道的最大效益-在儲存體和儲存工作負載叢集中。 這可讓工作負載叢集 (Hyper-v、SQL Server 容錯移轉叢集實例、儲存體複本等 ) 使用 SMB 多重通道，並更有效率地使用網路。 在交集 (也稱為分類式) 叢集設定，其中向外延展檔案伺服器叢集用來儲存 Hyper-v 或 SQL Server 容錯移轉叢集實例叢集的工作負載資料，此網路通常稱為「北南部子網」/網路。 許多客戶會藉由投資支援 RDMA 的 NIC 卡和交換器，將此網路的輸送量最大化。

![北南部 SMB 子網 ](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig2.png)
 **圖2：若要達到最大網路輸送量，請在向外延展檔案伺服器叢集和 hyper-v 或 SQL Server 容錯移轉叢集實例叢集上使用多個 nic-這會共用北南部子網**

![在相同子網中使用多個 Nic 的兩個叢集螢幕擷取畫面，以利用 SMB 多重通道 ](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig3.png)
 **圖3：兩個叢集 (用於儲存的向外延展檔案伺服器，SQL Server <abbr title=" 容錯移轉叢集實例 "> FCI 以 </abbr> 用於工作負載) 兩者都使用相同子網中的多個 nic 來利用 smb 多重通道並達到更佳的網路輸送量。**

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>IPv6 連結本機私人網路的自動識別
當私人 (叢集僅) 偵測到多個 Nic 的網路時，叢集將會自動辨識每個子網上每個 NIC 的 IPv6 連結本機 (fe80) 的 IP 位址。 這會節省系統管理員的時間，因為他們不再需要手動設定 IPv6 連結本機 (fe80) IP 位址資源。

使用一個以上的私人 (叢集僅) 網路時，請檢查 IPv6 路由設定，以確定路由未設定為跨子網，因為這會降低網路效能。

![容錯移轉叢集管理員 UI 中的自動網路設定螢幕擷取畫面 ](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig4.png)
 **圖4：自動 IPv6 連結本機 (Fe80) 位址資源**設定

## <a name="throughput-and-fault-tolerance"></a>輸送量和容錯
Windows Server 2019 和 Windows Server 2016 會自動偵測 NIC 功能，並會嘗試在最快的可能設定中使用每個 NIC。 您可以使用組合的 Nic、使用 RSS 的 nic，以及具備 RDMA 功能的 Nic。 下表摘要說明使用這些技術時的取捨。 使用多個支援 RDMA 的 Nic 時，會達到最大輸送量。 如需詳細資訊，請參閱[SMB Mutlichannel 的基本概念](/archive/blogs/josebda/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0)。

![各種 NIC 設定的輸送量和容錯的圖例 ](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig5.png)
 **圖5：各種 nic Conifigurations 的輸送量和容錯**能力

## <a name="frequently-asked-questions"></a>常見問題集
**多個 NIC 網路中的所有 Nic 是否都用於叢集核心訊號？**
是。

**多個 NIC 網路是否只能用於叢集通訊？或只能用於用戶端和叢集通訊？**
這兩種設定都可以使用-所有叢集網路角色都可以在多個 NIC 的網路上使用。

**SMB 多重通道是否也用於 CSV 和叢集流量？**
是，根據預設，所有叢集和 CSV 流量都會使用可用的多個 NIC 網路。 系統管理員可以使用容錯移轉叢集 PowerShell Cmdlet 或容錯移轉叢集管理員 UI 來變更網路角色。

**如何查看 SMB 多重通道設定？**
使用**SMBServerConfiguration** Cmdlet，尋找 EnableMultiChannel 屬性的值。

**叢集通用屬性 PlumbAllCrossSubnetRoutes 是在多個 NIC 網路上遵守嗎？**
是。

## <a name="additional-references"></a>其他參考資料
- [Windows Server 容錯移轉叢集的新功能](whats-new-in-failover-clustering.md)
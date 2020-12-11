---
description: 深入瞭解：簡化的 SMB 多重通道和多個 NIC 的叢集網路
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: 簡化 SMB 多重通道和多個 NIC 的叢集網路
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 537c5339505a57992c702d343bf9e130ced8d185
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047236"
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>簡化 SMB 多重通道和多個 NIC 的叢集網路

> 適用於：Windows Server 2019、Windows Server 2016

簡化的 SMB 多重通道和多重<abbr title="網路介面卡">NIC</abbr> 叢集網路是一項功能，可讓您在相同的叢集網路子網上使用多個 Nic，並自動啟用 SMB 多重通道。

簡化的 SMB 多重通道和多個 NIC 叢集網路提供下列優點：
- 容錯移轉叢集會在使用相同交換器/相同子網的節點上自動辨識所有 Nic-不需要額外的設定。
- SMB 多重通道會自動啟用。
- 只有 IPv6 連結本機 (fe80) IP 位址資源的網路會在僅限叢集 (私用) 網路上辨識。
- 預設會在每個叢集存取點上設定單一 IP 位址資源， (CAP) 網路名稱 (NN) 。
- 在相同的子網上找到多個 Nic 時，叢集驗證不再發出警告訊息。

## <a name="requirements"></a>需求
-   每一伺服器的多個 Nic，使用相同的交換器/子網。

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>如何利用多個 NIC 叢集網路和簡化的 SMB 多重通道
本節說明如何利用新的多個 NIC 叢集網路和簡化的 SMB 多重通道功能。

### <a name="use-at-least-two-networks-for-failover-clustering"></a>使用至少兩個網路進行容錯移轉叢集
雖然很罕見，但網路交換器可能會失敗，但最佳做法是至少針對容錯移轉叢集使用兩個網路。 所有找到的網路都會用於叢集的心跳。 避免針對您的容錯移轉叢集使用單一網路，以避免發生單一失敗點。 在理想情況下，叢集中的節點之間應該有多個實體通訊路徑，而且不會有單一失敗點。

![兩個用於容錯移轉叢集的網路 ](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig1.png)
 **圖1：針對容錯移轉叢集使用至少兩個網路**

### <a name="use-multiple-nics-across-clusters"></a>跨叢集使用多個 Nic

當您在儲存體和儲存工作負載叢集中跨叢集使用多個 Nic 時，可以達到簡化 SMB 多重通道的最大效益。 這可讓工作負載叢集 (Hyper-v、SQL Server 容錯移轉叢集實例、儲存體複本等） ) 使用 SMB 多重通道，並導致更有效率地使用網路。 在交集的 (也稱為分類式) 叢集設定，其中向外延展檔案伺服器叢集用於儲存 Hyper-v 或 SQL Server 容錯移轉叢集實例叢集的工作負載資料，此網路通常稱為「North-South 子網」/網路。 許多客戶藉由投資支援 RDMA 的 NIC 卡和交換器，將此網路的輸送量最大化。

![North-South SMB 子網 ](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig2.png)
 **圖2：若要達到最大的網路輸送量，請在擴充檔案伺服器叢集和 hyper-v 或 SQL Server 容錯移轉叢集實例叢集（共用 North-South 子網）上使用多個 nic。**

![使用相同子網中的多個 Nic 螢幕擷取畫面兩個叢集以利用 SMB 多重通道 ](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig3.png)
 **圖3：兩個叢集 (用於儲存的向外延展檔案伺服器、SQL Server <abbr title=" 容錯移轉叢集實例 "> FCI </abbr> 的工作負載) 兩者都會使用相同子網中的多個 nic 來利用 SMB 多重通道，並達到更高的網路輸送量。**

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>IPv6 連結本機私人網路的自動識別
當私人 (叢集僅) 偵測到具有多個 Nic 的網路時，叢集會自動辨識每個子網上每個 NIC 的 IPv6 連結本機 (fe80) IP 位址。 這可節省系統管理員的時間，因為他們不再需要手動設定 IPv6 連結本機 (fe80) IP 位址資源。

使用一個以上的私人 (叢集) 網路時，請檢查 IPv6 路由設定，以確保路由不會設定為跨子網，因為這樣會降低網路效能。

![容錯移轉叢集管理員 UI 中的自動網路設定螢幕擷取畫面 ](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig4.png)
 **圖4：自動 IPv6 連結本機 (Fe80) 位址資源** 設定

## <a name="throughput-and-fault-tolerance"></a>輸送量和容錯
Windows Server 2019 和 Windows Server 2016 會自動偵測 NIC 功能，並嘗試在最快的設定中使用每個 NIC。 組合的 Nic、使用 RSS 的 Nic，以及具有 RDMA 功能的 Nic 都可以使用。 下表摘要說明使用這些技術時的取捨。 使用多個支援 RDMA 的 Nic 時，可達到最大輸送量。 如需詳細資訊，請參閱 [SMB 多重通道的基本概念](/archive/blogs/josebda/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0)。

![各種 NIC 設定的輸送量和容錯的圖例 ](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig5.png)
 **圖5：各種 nic 設定的輸送量和容錯** 能力

## <a name="frequently-asked-questions"></a>常見問題集
**多個 NIC 網路中的所有 Nic 都是用於叢集核心訊號嗎？**
是。

**多個 NIC 網路只能用於叢集通訊嗎？也可以只用于用戶端和叢集通訊嗎？**
這兩種設定都可以運作-所有叢集網路角色都可在多個 NIC 的網路上運作。

**SMB 多重通道也用於 CSV 和叢集流量嗎？**
是，根據預設，所有叢集和 CSV 流量都會使用可用的多個 NIC 網路。 系統管理員可以使用容錯移轉叢集 PowerShell Cmdlet 或容錯移轉叢集管理員 UI 來變更網路角色。

**如何查看 SMB 多重通道設定？**
使用 **SMBServerConfiguration 指令程式** ，尋找 **EnableMultiChannel** 屬性的值。

**叢集通用屬性 PlumbAllCrossSubnetRoutes 是否遵守多重 NIC 網路？**
是。

## <a name="additional-references"></a>其他參考資料
- [Windows Server 容錯移轉叢集的新功能](whats-new-in-failover-clustering.md)

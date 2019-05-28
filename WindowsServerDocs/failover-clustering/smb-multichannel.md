---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: 簡化 SMB 多重通道和多個 NIC 的叢集網路
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 1b9271ceac99ac9b21cbfac902ba133d66815df4
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476119"
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>簡化 SMB 多重通道和多個 NIC 的叢集網路

> 適用於：Windows Server 2019，Windows Server 2016

簡化 SMB 多重通道和多-<abbr title="網路介面卡">NIC</abbr>叢集網路，是一項功能，可讓您使用多個 Nic 位於相同的叢集網路子網路，並會自動啟用 SMB 多重通道。

簡化 SMB 多重通道和多個 NIC 的叢集網路提供下列優點：  
- 容錯移轉叢集會自動識別節點所使用的相同交換器上的所有 Nic / 相同子網路-不需要另外設定。  
- SMB 多重通道會自動啟用。  
- 只有 IPv6 連結本機 （fe80 開頭） 的 IP 位址資源的網路會辨識叢集專用的 （私人） 網路上。  
- 單一 IP 位址資源是預設設定在每個叢集存取點 (CAP) 網路名稱 (NN)。  
- 相同的子網路上找到多個 Nic 時，不再叢集驗證就會發出警告訊息。  

## <a name="requirements"></a>需求  
-   使用相同的交換器每一部伺服器，多個 Nic / 子網路。  

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>如何充分利用多個 NIC 的叢集網路 」 和 「 簡化的 SMB 多重通道  
本節說明如何利用新的多個 NIC 的叢集網路 」 和 「 簡化的 SMB 多通路功能。  

### <a name="use-at-least-two-networks-for-failover-clustering"></a>使用至少兩個網路的容錯移轉叢集   
雖然很罕見，但可能會失敗的網路交換器-仍最佳做法是使用容錯移轉叢集的兩個以上的網路。 找到的所有網路會都用於叢集活動訊號。 請避免使用容錯移轉叢集的單一網路，以避免單一失敗點。 在理想情況下，應該會有多個實體的通訊路徑在叢集中節點之間任何單一失敗點。  

![容錯移轉叢集的兩個網路的圖例說明](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig1.png)  
**圖 1:使用至少兩個網路的容錯移轉叢集**  

### <a name="use-multiple-nics-across-clusters"></a>使用多個叢集的多個 Nic  

簡化 SMB 多重通道的最大優點是在跨叢集-儲存體工作負載的叢集和儲存體中使用多個 Nic 時達成的。 這可以讓網路更有效率地使用工作負載有更多的叢集 （HYPER-V、 SQL Server 容錯移轉叢集執行個體、 儲存體複本等） 使用 SMB 多重通道和結果。 在交集，（也稱為分類式） 叢集的組態中，其中的向外延展檔案伺服器叢集用於儲存 HYPER-V 工作負載資料，或 SQL Server 容錯移轉叢集執行個體叢集中，此網路通常稱為 「 北南向調整大小的子網路 」，/網路. 許多客戶的這個網路輸送量最大化藉由投資 RDMA 功能的 nic 介面卡和交換器。  

![北-南 SMB 子網路的圖例](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig2.png)  
**圖 2:若要達到最大網路輸送量，請向外延展檔案伺服器叢集和 HYPER-V 或 SQL Server 容錯移轉叢集執行個體叢集-共用北南向調整大小的子網路上使用多個 Nic**  

![使用相同的子網路中的多個 Nic，利用 SMB 多重通道的兩個叢集的螢幕擷取畫面](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig3.png)  
**圖 3:兩個群集 (儲存體、 SQL Server 的向外延展檔案伺服器<abbr title="容錯移轉叢集執行個體">FCI</abbr>工作負載) 都使用相同的子網路中的多個 Nic，利用 SMB 多重通道，並達成更好的網路輸送量。** 

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>自動辨識 IPv6 連結本機的私人網路  
當偵測到具有多個 Nic 的私人 （只在叢集） 網路，則叢集會自動辨識 IPv6 連結本機 （fe80 開頭） 的 IP 位址的每個 NIC 每個子網路上。 將系統管理員此時因為他們不再需要手動設定 IPv6 連結本機 （fe80 開頭） 的 IP 位址資源。  

使用一個以上的私用 （只在叢集） 網路時，請檢查以確保路由不設定跨子網路，因為這會降低網路效能的 IPv6 路由組態。  

![在容錯移轉叢集管理員 UI 中的自動網路設定的螢幕擷取畫面](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig4.png)  
**圖 4:自動 IPv6 連結本機 (fe80) 位址的資源設定**  

## <a name="throughput-and-fault-tolerance"></a>輸送量和容錯功能  
Windows Server 2019 和 Windows Server 2016 自動偵測 NIC 功能，並會嘗試使用最快的可能組態中的每個 NIC。 會組合使用介面卡的 Nic、 Nic 使用 RSS，並具備 RDMA 功能的 Nic 都可以用。 下表摘要說明取捨時使用這些技術。 使用多個 RDMA Nic 時，最大輸送量是來達成。 如需詳細資訊，請參閱 < [SMB Mutlichannel 的基本概念](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/)。

![舉例說明如何針對各種不同的 NIC 組態的輸送量和容錯](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig5.png)  
**圖 5:針對各種不同的 NIC conifigurations 負載的輸送量和容錯功能**   

## <a name="frequently-asked-questions"></a>常見問題集  
**在多個 NIC 的網路中的所有網路介面卡會用於叢集訊號？**  
    是的。  

**多個 NIC 的網路會用於叢集通訊？或者，它僅適用於用戶端和叢集通訊？**  
    請設定將運作-所有的叢集網路角色能夠在多個 NIC 的網路上。  

**SMB 多重通道也會用於 CSV 和叢集的流量？**  
    是，根據預設所有叢集和 CSV 流量會都使用可用的多個 NIC 的網路。 系統管理員可以使用容錯移轉叢集 PowerShell cmdlet 或容錯移轉叢集管理員 UI 來變更網路角色。  

**如何查看 SMB 多重通道設定？**  
    使用**Get SMBServerConfiguration** cmdlet，尋找 EnableMultiChannel 屬性的值。  

**在多個 NIC 的網路上是 PlumbAllCrossSubnetRoutes 遵守叢集一般屬性？**  
     是的。  

## <a name="see-also"></a>另請參閱  
- [容錯移轉叢集的 Windows Server 最新消息](whats-new-in-failover-clustering.md)  

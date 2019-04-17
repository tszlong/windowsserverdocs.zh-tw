---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: "簡化的 SMB 多頻道，使用多監視器-NIC 叢集網路"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 45d8364adf9d3db24a8e6d8f7bc91178ce7d1551
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>簡化的 SMB 多頻道，使用多監視器-NIC 叢集網路

> 適用於：Windows Server（以每年次通道）、Windows Server 2016

簡化 SMB 多頻道和多-<abbr title="[網路介面卡">NIC</abbr>叢集網路是在 Windows Server 2016 的允許使用的相同叢集子網路，在多個 Nic，並自動讓 SMB 多頻道中的新功能。  

簡化 SMB 多頻道，使用多監視器-NIC 叢集網路優點下列動作：  
- 自動容錯辨識所有 Nic 所使用的同一個開關切換至節點上 / 相同子網路-不需要額外的設定。  
- SMB 多頻道時自動支援。  
- 只有 IPv6 連結本機 (fe80) IP 位址資源網路辨識僅限叢集 （私人） 網路上。  
- 單一 IP 位址資源預設被設定在每個叢集存取點 （端點） 網路名稱 (NN)。  
- 多個 Nic 發現在相同的子網路時，不再叢集驗證問題警告訊息。  

## <a name="requirements"></a>需求  
-   多個 Nic 每個伺服器，使用相同的參數日子網路。  

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>如何利用多-NIC 叢集網路與簡化的 SMB 多頻道  
本章節告訴您如何利用 Windows Server 2016 中的新多重-NIC 叢集網路與簡化的 SMB 多頻道功能。  

### <a name="use-at-least-two-networks-for-failover-clustering"></a>使用兩個以上網路容錯   
雖然少見，但可能會失敗網路參數-最好還是使用兩個以上網路容錯。 用於叢集活動訊號找到所有的網路。 避免使用單一容錯移轉叢集的網路，以避免失敗的單點。 理論上，應該會有多個實體的通訊路徑叢集、 節點和失敗的單點之間。  

![有兩個網路的圖例中容錯](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig1.png)  
**圖 1： 容錯使用兩個以上網路**  

### <a name="use-multiple-nics-across-clusters"></a>在群集上使用多個 Nic  

多個 Nic 叢集-的儲存空間工作負載叢集和存放裝置上使用時，方法是最多的優點簡化 SMB 多頻道。 這樣工作負載叢集 （HYPER-V、 SQL Server 容錯移轉叢集執行個體、 儲存複本等），並結果 SMB 多頻道中的網路使用更有效率。 在聚合型 (也稱為 disaggregated) 叢集的設定位置延展檔案伺服器叢集用於工作負載的資料儲存於 HYPER-V 的或 SQL Server 容錯移轉叢集執行個體叢集，這個網路通常稱為 「 北南子網路 「 / 網路。 許多針對放到最大這個網路的輸送量 RDMA 可 nic 介面卡和參數投資下去。  

![北南 SMB 子網路的圖例](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig2.png)  
**圖 2 所示： 達成網路最大的輸送量，使用多個 Nic HYPER-V 或 SQL Server 容錯移轉叢集執行個體叢集-共用北南子網路和延展檔案伺服器叢集上**  

![這兩個叢集使用相同的子網路中的多個 Nic 利用 SMB 多頻道的 Screencap](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig3.png)  
**圖 3 所示： 這兩個叢集 (延展檔案伺服器的儲存空間，SQL Server<abbr title="容錯移轉叢集執行個體">FCI</abbr>工作負載) 兩者都使用相同的子網路中的多個 Nic 運用 SMB 多頻道並獲得更好的網路輸送量。** 

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>自動辨識 IPv6 連結本機私人網路  
偵測到多個 Nic （僅限叢集） 私人網路時叢集將會自動辨識 IPv6 連結區域 (fe80) IP 位址的每個 NIC 每個子網路上。 這個節省系統管理員的時間之後，他們就不需要再手動設定資源 IPv6 連結本機 (fe80) IP 位址。  

當您使用多個 （僅限叢集） 私人網路，請檢查 IPv6 路由設定，以確保的路由並未設定成跨子網路，因為這將會減少網路效能。  

![Screencap 的容錯移轉叢集管理員 UI 中自動網路設定](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig4.png)  
**圖 4： 自動 IPv6 連結區域 (fe80) 的位址資源設定**  

## <a name="throughput-and-fault-tolerance"></a>輸送量和容錯  
Windows Server 2016 自動偵測到 NIC 功能，並嘗試每個 NIC 使用最快速可能的設定。 Nic 的，以、 Nic 使用 RSS，以及 Nic RDMA 功能的所有可用。 下表摘要折衷使用這些技術。 最大的輸送量，達成時使用多個 RDMA 可 Nic。 如需詳細資訊，請查看[SMB Mutlichannel 的基本知識](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/)。

![輸送量和錯誤容錯不同 NIC 設定的圖例](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig5.png)  
**適用於各種 NIC conifigurations 圖 5 所示︰ 輸送量和錯誤容錯**   

## <a name="frequently-asked-questions"></a>常見問題集  
**多重-NIC 網路中的所有 Nic 都用於叢集心打嗎？**  
    [是]。  

**多重-NIC 網路可用於叢集通訊嗎？ 或它僅能 client 和叢集通訊嗎？**  
    不論是設定能-所有叢集網路角色都能使用多監視器-NIC 網路上。  

**SMB 多頻道也可用於 CSV 和叢集流量嗎？**  
    是的預設所有叢集和 CSV 流量會都使用可用的多 NIC 網路。 系統管理員可以使用容錯移轉叢集 PowerShell cmdlet 或容錯移轉叢集管理員 UI 變更的網路角色。  

**如何可以查看多 SMB 頻道設定？**  
    使用**取得-SMBServerConfiguration** cmdlet，尋找 EnableMultiChannel 屬性的值。  

**是叢集常見屬性 PlumbAllCrossSubnetRoutes 遵守多-NIC 網路上？**  
     [是]。  

## <a name="see-also"></a>也了  
- [Windows Server 容錯中的新功能](whats-new-in-failover-clustering.md)  

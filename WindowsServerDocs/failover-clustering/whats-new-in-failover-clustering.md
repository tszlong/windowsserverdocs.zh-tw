---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: "在 Windows Server 容錯中的新功能"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: dongill
author: JasonGerend
ms.author: jgerend
ms.date: 10/11/2016
ms.openlocfilehash: a4330f62095e13f2f4736f15924ed31fb4893e7a
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="whats-new-in-failover-clustering-in-windows-server-2016"></a>在 Windows Server 2016 容錯中的新功能
> 適用於：Windows Server（以每年次通道）、Windows Server 2016

本主題解釋新功能和變更功能在 Windows Server 2016 中容錯。  

## <a name="BKMK_RollingUpgrade"></a>升級叢集作業系統  
新功能，容錯，叢集作業系統循環升級，讓系統管理員可以從 Windows Server 2012 R2 升級作業系統叢集節點以 Windows Server 2016 不停止 HYPER-V 或延展檔案伺服器工作負載。 使用這項功能，可避免當機損失針對服務層級合約 (SLA)。  

**這項變更新增值為何？**  

升級 HYPER-V 或延展檔案伺服器叢集 Windows Server 2012 R2 的 Windows Server 2016 不需要中斷。 叢集將繼續運作在 Windows Server 2012 R2 的層級，直到所有節點叢集在執行 Windows Server 2016。 使用 Windows PowerShell cmdlt 叢集功能層級升級至 Windows Server 2016 `Update-ClusterFunctionalLevel`。  

> [!WARNING]  
> -   在更新的叢集功能層級之後，您無法回復到 Windows Server 2012 R2 叢集功能層級。  
> -   直到`Update-ClusterFunctionalLevel`執行 cmdlet、 程序會還原，Windows Server 2012 R2 節點新增及 Windows Server 2016 節點可以移除。  

**有哪些方式各不相同？**  

HYPER-V 或延展檔案伺服器容錯移轉叢集可以立即輕鬆升級而不需要任何當機或需要建立新的叢集與執行 Windows Server 2016 作業系統節點。 Windows Server 2012 R2 移轉叢集涉及 offline 拍攝現有叢集並重新安裝新的作業系統的每個節點，然後將叢集回。 舊程序是麻煩且需要中斷。 不過，Windows Server 2016 中叢集不必離線隨時。  

叢集作業系統升級階段中的每個節點叢集如下所示：  
-   節點為暫停，並耗盡的所有虛擬電腦上執行。  
-   移轉到另一個節點叢集虛擬機器 （或其他叢集工作負載）。移轉到另一個節點叢集虛擬的電腦。  
-   在移除現有的作業系統，並執行全新安裝的 Windows Server 2016 上的作業系統節點。  
-   執行 Windows Server 2016 operating systems\] 節點中新增了返回叢集。  
-   此時，叢集稱為執行混合模式，因為叢集節點執行 Windows Server 2012 R2 或 Windows Server 2016。  
-   叢集功能層級停留在 Windows Server 2012 R2。 在此功能的層級，將無法使用 Windows Server 2016 影響的舊版作業系統的相容性的新功能。  
-   最後，所有節點被都升級到 Windows Server 2016。  
-   Windows Server 2016 使用 Windows PowerShell cmdlet 然後變更叢集功能等級`Update-ClusterFunctionalLevel`。 此時，您可以利用 Windows Server 2016 的功能。  

如需詳細資訊，請查看[叢集作業系統循環升級](cluster-operating-system-rolling-upgrade.md)。  

## <a name="BKMK_SR"></a>儲存空間複本  
儲存複本是新的功能，可讓無關儲存空間、 封鎖層級，同步複寫之間伺服器或叢集復原嚴重損壞，以及延伸容錯移轉叢集間的網站。 同步複寫讓鏡像實體的網站，以確保檔案系統層級 0 資料遺失當機一致磁碟區中的資料。 非同步複寫可以外地區的範圍資料遺失的可能性網站擴充功能。  

**這項變更新增值為何？**  

儲存空間複本可讓您執行下列動作：  

-   提供單一計劃與非預期關閉任務重要工作負載的供應商損壞修復方案。  

-   使用 SMB3 傳輸經證實的可靠性、 延展性和效能。  

-   延伸到城市距離 Windows 容錯。  

-   使用 Microsoft 軟體端點儲存和叢集、 HYPER-V、 複本儲存空間、 儲存空間、 叢集、 延展檔案伺服器、 SMB3、 資料 Deduplication，和 NTFS ReFS 日。  

-   協助降低成本及複雜，如下所示：  

    -   是硬體診斷，例如 DAS 或舊的特定的儲存空間設定不需要的。  

    -   可讓商品儲存和網路技術。  

    -   容易圖形個人節點和叢集透過容錯移轉叢集管理員管理功能。  

    -   包含完整、 大規模透過 Windows PowerShell 指令碼的選項。  

-   協助降低當機，以及提高可靠性和生產力建 windows。  

-   提供性，效能計量，以及診斷功能。  

如需詳細資訊，請查看[在 Windows Server 2016 Storage 複本](../storage/storage-replica/storage-replica-overview.md)。  


## <a name="BKMK_CloudWitness"></a>雲端見證  
雲端見證是容錯移轉叢集仲裁見證使用 Microsoft Azure 仲裁點的 Windows Server 2016 中的新類型。 雲端見證，例如其他仲裁見證取得投票和參與的仲裁計算。 您可以將雲端見證設定為使用設定叢集仲裁精靈仲裁見證。  

**這項變更新增值為何？**  

使用雲端見證容錯移轉叢集仲裁見證提供下列優點：  

-   Microsoft Azure 會使用，不需要第三個不同的資料中心。  

-   請使用標準公開使用 Microsoft Azure Blob 儲存額外維護減少 Vm 裝載公開雲端中的。  

-   相同 Microsoft Azure 儲存 Account 可用於多個叢集 （每個叢集的一個大型物件檔案; 叢集唯一 id 做為 blob 檔案名稱）。  

-   儲存空間過去 （每個大型物件檔案，撰寫大型物件檔案更新一次叢集節點狀態時變更非常小資料） 提供極低持續成本。  

如需詳細資訊，請查看[部署雲端見證的容錯移轉叢集](deploy-cloud-witness.md)。  

**有哪些方式各不相同？**  

這項功能的 Windows Server 2016 中的新功能。  

## <a name="BKMK_VMs"></a>一樣恢復  
**計算恢復**Windows Server 2016 包含提升的虛擬電腦運算恢復功能以協助降低運算叢集站叢集間通訊問題，如下所示： 

-   **靈活度選項適用於虛擬電腦：**您現在可以設定定義暫時性失敗時的行為虛擬機器一樣恢復選項：  

    -   **靈活度層級︰**可協助您定義暫時性失敗的處理方式。  

    -   **靈活度期間：**可協助您定義多久虛擬機器允許執行的隔離。  

-   **隔離的健康節點：**不良節點隔離，以及不允許加入叢集。 如此可防止拍打節點不良影響的整體叢集及其他節點。  

更多的資訊一樣運算恢復工作流程和節點隔離設定控制如何將您的節點放在隔離或隔離，請查看[在 Windows Server 2016 一樣計算恢復](http://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx)。  

**儲存空間恢復**在 Windows Server 2016 虛擬機器是援引暫時性儲存失敗。 改善的一樣恢復功能可協助保留承租人一樣工作階段美國萬一儲存干擾。 這是由儲存基礎結構問題的智慧與快速一樣回應達成。  

時一樣中斷為基礎的儲存空間，它暫停，並等候儲存復原。 而暫停，一樣會保留執行中應用程式。 還原其儲存至一樣的連接後，一樣回到執行的狀態。 如此一來，承租人電腦的工作階段狀態保留修復上。  

在 Windows Server 2016 一樣儲存靈活度和最佳化來賓叢集也是。  

## <a name="BKMK_Diagnostics"></a>診斷容錯改進  
為了診斷問題容錯，Windows Server 2016 如下：  

-   （例如時區資訊和 DiagnosticVerbose 登入） 叢集登入檔案，讓數個改進很容易地容錯移轉叢集問題進行疑難排解。 如需詳細資訊，請查看[Windows Server 2016 容錯移轉叢集疑難排解調節-叢集登入](http://blogs.msdn.com/b/clustering/archive/2015/05/15/10614930.aspx)。  

-   新傾印輸入的**使用中的記憶體傾印**，篩選掉大部分的記憶體頁配置虛擬電腦，以及很多較小、 儲存，或是複製更容易，因此可 memory.dmp。  如需詳細資訊，請查看[Windows Server 2016 容錯移轉叢集疑難排解調節-主動式傾印](http://blogs.msdn.com/b/clustering/archive/2015/05/18/10615526.aspx)。  

## <a name="BKMK_SiteAware"></a>網站感知容錯  
Windows Server 2016 包含網站-注意容錯，讓他們所在位置 （網站） 為基礎的延伸叢集群組節點。 叢集網站-感知美化金鑰作業叢集週期，例如錯誤後移轉的行為，位置原則活動訊號之間的節點和仲裁行為。 如需詳細資訊，請查看[在 Windows Server 2016 中的網站感知容錯](http://blogs.msdn.com/b/clustering/archive/2015/08/19/10636304.aspx)。  

## <a name="BKMK_multidomainclusters"></a>工作群組和多網域叢集  
在 Windows Server 2012 R2，舊版叢集只能加入網域相同的成員節點間建立。   Windows Server 2016 中斷下這些障礙和導入建立容錯移轉叢集不 Active Directory 相依性的能力。 您現在可以建立容錯依照以下設定：  

-   **單一網域叢集。** 加入網域相同的所有節點以群集。  

-   **多網域叢集。** 群集的節點就是不同的網域的成員。  

-   **工作群組叢集。** 群集的節點就是成員伺服器日群組 （不加入的網域）。  

如需詳細資訊，請查看[在 Windows Server 2016 中的工作群組和多網域叢集](http://blogs.msdn.com/b/clustering/archive/2015/08/17/10635825.aspx)  
## <a name="BKMK_VMLoadBalancing"></a>一樣負載平衡  
一樣負載平衡是容錯幫助您順暢負載平衡虛擬電腦的跨節點叢集中的新功能。 根據一樣記憶體和節點上的 CPU 使用率都會覆致力的節點。 （即時移轉） 然後移動虛擬電腦覆致力節點提供頻寬 （如果有的話）。 平衡侵略可以調整至確保叢集最佳效能與使用。  Windows 伺服器 2016 Technical Preview 中的預設會讓負載平衡。 不過，負載平衡已停用時可以 SCVMM 動態最佳化。  

## <a name="BKMK_VMStartOrder"></a>[開始] 畫面一樣的訂單  
一樣開始訂單位於叢集的容錯引進虛擬電腦的 [開始] 畫面訂單協調流程 （和所有的群組） 中的新功能。 虛擬的電腦現在可分成層級，和 [開始] 畫面順序相依性可以建立之間的不同層級。 這樣可確保最重要的虛擬電腦 （例如網域控制站或公用程式虛擬機器） 開始是第一次。 虛擬的電腦不被開始之前他們對相依性虛擬電腦也會開始。  

## <a name="BKMK_SMBMultiChannel"></a>簡化的 SMB 多頻道，使用多監視器-NIC 叢集網路  
容錯移轉叢集網路不再是每個子網路的單一 NIC / 網路。 簡化 SMB 多頻道，使用多監視器-NIC 叢集網路網路設定為自動與每個 NIC 子網路上的可以用於叢集和工作負載的資料傳輸。 這個 enhancement 可針對來最大化 HYPER-V、 SQL Server 容錯移轉叢集執行個體與其他 SMB 工作負載的網路輸送量。  

如需詳細資訊，請查看[簡體 SMB 多頻道，使用多監視器-NIC 叢集網路](smb-multichannel.md)。

## <a name="see-also"></a>也了  
* [儲存空間](../storage/storage.md)  
* [Windows Server 2016 中的儲存空間中的新功能](../storage/whats-new-in-storage.md)  

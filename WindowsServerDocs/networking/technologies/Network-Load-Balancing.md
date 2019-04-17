---
title: 網路負載平衡
description: 本主題提供的網路負載平衡 (NLB) 的功能在 Windows Server 2016 概觀，並包含額外的指導方針建立、設定及管理 NLB 群集的相關的連結。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-nlb
ms.topic: article
ms.assetid: 244a4b48-06e5-4796-8750-a50e4f88ac72
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b9fd39381316a8bcd06328e7aa75492ed99bc7f1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="network-load-balancing"></a>網路負載平衡

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題提供的網路負載平衡 \(NLB\) 功能在 Windows Server 2016 概觀，並包含額外的指導方針建立、設定及管理 NLB 群集的相關的連結。

您可以使用 NLB 做為單一 virtual 叢集管理兩個或更多的伺服器。 NLB 美化的可用性和網際網路伺服器應用程式，例如 FTP 網頁上所使用的延展性、防火牆、proxy，virtual 私人網路 \(VPN\)，以及其他重要 mission\ 的伺服器。   

>[!NOTE]
>Windows Server 2016 包含新就 Azure-迫不及待軟體負載平衡器 \(SLB\) 元件的軟體定義網路 \(SDN\) 基礎結構。 使用 SLB NLB 而如果您使用 SDN，使用非 Windows 工作負載，需要輸出網路位址轉譯 \(NAT\)，或者需要層級 3 \(L3\) 或非 TCP 根據負載平衡。 您可以繼續使用與 Windows Server 2016 NLB 非 SDN 部署。 如需 SLB 的詳細資訊，請查看[軟體負載平衡 (SLB) SDN 的](../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。

流量分配數個伺服器、使用 TCP\ 日 IP 網路通訊協定網路負載平衡 \(NLB\) 功能。 藉由組合兩個或更多的電腦正在執行的應用程式成單一的 virtual 叢集，NLB 提供的可靠性與 web 伺服器效能和其他 mission\ 重大伺服器。  
  
在 NLB 叢集伺服器稱為*主機*，與每個主機執行獨立伺服器應用程式的複本。 NLB 分散叢集主機的收到 client 的要求。 您可以設定為每個主機處理載入。 您也可以新增處理負載增加到叢集動態主機。 NLB 也可以直接所有的資料傳輸到指定的單一主機，稱為*預設主機*。  
  
NLB 可讓所有電腦的 IP 位址，來處理叢集，並為每個主機保持的唯一、專用 IP 位址設定。 Load\ 平衡應用程式時主機失敗或進入] 載入是自動轉散發之間仍會運作的電腦。 準備好時，離線電腦無障礙重新叢集並重新允許的其他電腦中叢集處理較少的流量的工作負載的。  
  
## <a name="BKMK_APP"></a>實用的應用程式  
NLB 適合用來確保無應用程式，例如執行 \(IIS\) 的網頁伺服器可提供最少當機，並的延展性 \（透過新增額外的伺服器載入 increases\ 為）。 下列章節描述 NLB 如何支援可用性、延展性及管理性執行這些應用程式的叢集伺服器。  
  
### <a name="high-availability"></a>可用性  
可用性系統可靠地提供最少中斷與服務可接受層級。 為了提供可用性，NLB 包含 built\ 中的功能可自動：  
  
-   偵測叢集主機失敗或進入]，然後進行復原。  
  
-   新增或移除主機時會網路負載平衡。  
  
-   復原，將套件轉工作負載中 10 秒。  
  
### <a name="scalability"></a>擴充性  
延展性是程度電腦、服務或應用程式可一起拓展符合增加效能要求評量。 用於叢集 NLB，延展性是新增一或多個系統累加現有叢集時的整體負載叢集超過該功能的能力。 若要支援延展性，您可以使用 NLB 下列：  
  
-   個人 TCP\ 日 IP 服務 NLB 叢集平衡載入要求。  
  
-   在單一叢集支援最多 32 電腦。  
  
-   平衡多個伺服器載入要求 \（從相同 client 或數個 clients\）叢集在多部主機上。  
  
-   新增到 NLB 叢集主機載入增加，而失敗叢集。  
  
-   時降低載入移除叢集主機。  
  
-   讓高效能和透過完整管線實作低費用。 管線允許要求傳送給 NLB 叢集而不想等待回應之前要求。  
  
### <a name="manageability"></a>管理性  
若要支援管理，您可以使用 NLB 下列：  
  
-   管理和使用 NLB 管理員設定多個 NLB 叢集和叢集主機從一部電腦或[Windows PowerShell 中的 [網路負載平衡 (NLB) Cmdlet](https://technet.microsoft.com/library/hh801274.aspx)。
  
-   指定負載平衡行為單一 IP 連接埠或群組的連接埠使用連接埠管理規則。  
  
-   定義網站，每個不同的連接埠的規則。 如果您使用多個應用程式或網站 load\ 平衡伺服器相同設定、連接埠規則為基礎的目的地 virtual IP 位址 \(using virtual clusters\)。  

-   直接存取所有的單一主機使用選擇性 client 請求、single\ 主機規則。 來執行特定應用程式的特定主機 NLB 路徑 client 要求。  

-   封鎖不想要的網路存取特定 IP 連接埠。  

-   讓 Internet 群組管理通訊協定 \(IGMP\) 支援控制切換連接埠流叢集主機 \（連入網路封包傳送到 switch\ 上所有的連接埠）時多點模式中的作業系統。  

-   [開始]、停止及使用 Windows PowerShell 命令或指令碼遠端控制 NLB 動作。  

-   檢視檢查 NLB 事件 Windows 事件登入。 NLB 登事件木頭中的所有動作和集群的變更。  

## <a name="important-functionality"></a>重要的功能  
 
安裝 NLB 為標準 Windows Server 網路驅動程式元件。 其作業的透明網路 TCP\ 日 IP 堆疊。 下圖顯示 NLB 和其他軟體元件之間的關係，一般設定中。  
  
![網路負載平衡和其他軟體元件](../media/NLB/nlb.jpg)  
  
以下是主要 NLB 的功能。  
  
- 您不需要硬體變更執行。  
  
- 提供的網路負載平衡工具來設定及管理多個叢集與所有主機的遠端或本機一部電腦。  
  
- 可透過單一的邏輯網際網路名稱及 virtual IP 位址，也就是叢集 IP 位址存取叢集戶端 \（保留 computer\ 每個人的名稱）。 NLB 多重主目錄伺服器允許多個 virtual IP 位址。  
  
> [!NOTE]  
> 當您部署 Vm virtual 叢集為時，NLB 不需要伺服器多重主目錄有多個 virtual IP 位址。  
  
- 可讓 NLB 結合多個網路介面卡，如此可讓您設定多個獨立叢集每個主機上。 多個網路介面卡的支援 mca virtual 叢集在於 virtual 叢集可讓您在單一網路介面卡設定多個叢集。  
  
- 需要不修改伺服器應用程式，讓他們可以執行 NLB 叢集。  
  
- 您可以設定自動新增到該叢集主機失敗，並會在接下來，如果叢集主機帶回上網。 新增的主機處理從新的伺服器要求就可以開始。  
  
-   可讓您拍攝 offline 電腦的預防維護而不會影響其他主機上的叢集作業。  
  
## <a name="BKMK_HARD"></a>硬體需求  
以下是執行 NLB 叢集的硬體需求。  
  
-   相同子網路上，必須位於叢集在所有主機。  
  
-   在每個主機網路介面卡的數目無限制，並不同的主機可以有許多不同的介面卡。  
  
-   每個群集，在所有網路介面卡都必須多點傳送或單點。 NLB 不支援多點傳送與單單一叢集在混合的環境。  
  
-   如果您使用的單模式，用來處理 client\ to\ 叢集流量的網路介面卡必須支援變更其媒體存取控制 \(MAC\) 位址。  
  
## <a name="BKMK_SOFT"></a>軟體需求  
以下是執行 NLB 叢集軟體需求。  
  
-   僅限 TCP\ 日 IP 可以用於 NLB 讓每個主機上的顯示卡。 無法新增任何其他通訊協定 \ (例如，IPX\) 這個介面卡。  
  
-   您必須靜態叢集伺服器的 IP 位址。  
  
> [!NOTE]  
> NLB 不支援動態主機設定通訊協定 \(DHCP\)。 NLB 停用該設定的每個介面上 DHCP。  
  
## <a name="BKMK_INSTALL"></a>安裝資訊  
您可以藉由伺服器管理員或的 Windows PowerShell 命令 NLB 安裝 NLB。

或者，您可以安裝網路負載平衡工具管理本機或遠端 NLB 叢集。 這些工具包含網路負載平衡管理員和 NLB Windows PowerShell 命令。

### <a name="installation-with-server-manager"></a>安裝與伺服器管理員

在伺服器管理員中，您可以使用 [新增角色與功能精靈新增**網路負載平衡**功能。 當您完成精靈時，和 NLB 已安裝的您不需要重新開機。


### <a name="installation-with-windows-powershell"></a>使用 Windows PowerShell 安裝  

若要使用 Windows PowerShell 來安裝 NLB，執行下列命令在已提升權限的 Windows PowerShell 命令提示字元在電腦上您想要安裝 NLB。

    
    Install-WindowsFeature NLB -IncludeManagementTools
    
安裝完成後的電腦不重新開機需要。

如需詳細資訊，請查看[安裝-WindowsFeature](https://technet.microsoft.com/library/jj205467.aspx)。

### <a name="network-load-balancing-manager"></a>網路負載平衡管理員
若要打開網路負載平衡管理員在伺服器管理員中，按一下 [**工具**，然後按**網路負載平衡管理員**。
  
## <a name="BKMK_LINKS"></a>其他資源  
下表提供 NLB」功能的其他資訊連結。  
  
|內容類型|資訊尋找參考資料|  
|----------------|--------------|  
|部署|[網路負載平衡部署指南](https://technet.microsoft.com/library/cc754833(WS.10).aspx)和 #124;[設定網路負載平衡終端服務](https://technet.microsoft.com/library/cc771300(v=WS.10).aspx)|  
|作業|[管理網路負載平衡叢集](https://technet.microsoft.com/library/cc753954(WS.10).aspx)和 #124;[[設定網路負載平衡參數](https://technet.microsoft.com/library/cc731619(WS.10).aspx)和 #124;[控制網路負載平衡叢集上的主機](https://technet.microsoft.com/library/cc770870(WS.10).aspx)|  
|疑難排解|[疑難排解網路負載平衡叢集](https://technet.microsoft.com/library/cc732592(WS.10).aspx)和 #124;[NLB 叢集事件與錯誤](https://technet.microsoft.com/library/cc731678(WS.10).aspx)|
|工具和設定|[網路負載平衡 Windows PowerShell cmdlet](https://go.microsoft.com/fwlink/p/?LinkId=238123)|
|社群資源|[可用性 \(Clustering\) 論壇](https://go.microsoft.com/fwlink/p/?LinkId=230641)
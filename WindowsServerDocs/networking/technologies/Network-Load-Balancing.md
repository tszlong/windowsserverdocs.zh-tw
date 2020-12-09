---
title: Network Load Balancing
description: 在本主題中，我們將概述 \( Windows Server 2016 中的網路負載平衡 NLB \) 功能。 您可以使用 NLB 將兩部以上的伺服器當作單一虛擬叢集來管理。 NLB 可增強網際網路伺服器應用程式（例如，用於 web、FTP、防火牆、proxy、虛擬私人網路 \( VPN \) 及其他任務關鍵性伺服器上）的可用性和擴充性 \- 。
manager: dougkim
ms.topic: article
ms.assetid: 244a4b48-06e5-4796-8750-a50e4f88ac72
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: 2edcd046958854698fbcc61d96f2716a26de3367
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866267"
---
# <a name="network-load-balancing"></a>Network Load Balancing

>適用於：Windows Server (半年度管道)、Windows Server 2016

在本主題中，我們將概述 \( Windows Server 2016 中的網路負載平衡 NLB \) 功能。 您可以使用 NLB 將兩部以上的伺服器當作單一虛擬叢集來管理。 NLB 可增強網際網路伺服器應用程式（例如，用於 web、FTP、防火牆、proxy、虛擬私人網路 \( VPN \) 及其他任務關鍵性伺服器上）的可用性和擴充性 \- 。

> [!NOTE]
> Windows Server 2016 包含新的 Azure 靈感軟體，Load Balancer \( SLB \) 作為軟體定義網路 \( SDN 基礎結構的元件 \) 。 如果您使用 SDN、使用非 Windows 工作負載、需要輸出網路位址轉譯 \( NAT \) ，或需要第3層 \( L3 \) 或非 TCP 型負載平衡，請使用 SLB 而非 NLB。 您可以繼續針對非 SDN 部署使用 Windows Server 2016 的 NLB。 如需 SLB 的詳細資訊，請參閱 [SDN)  (slb 的軟體負載平衡](../sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)。

網路負載平衡 \( NLB \) 功能會使用 TCP \/ IP 網路通訊協定，將流量分散到數部伺服器。 藉由將執行應用程式的兩部或多部電腦結合成單一虛擬叢集，NLB 可提供 web 伺服器與其他任務關鍵性伺服器的可靠性和效能 \- 。

NLB 叢集中的伺服器稱為「主機」，而且每部主機都會執行不同複本的伺服器應用程式。 NLB 將連入的用戶端要求分配給叢集中的各個主機。 您可以設定每部主機要處理的負載。 您也可以將主機動態新增至叢集，以處理增加的負載。 此外，NLB 可以將所有流量導向指定的單一主機，該主機稱為「預設主機」。

NLB 允許利用同一組 IP 位址將叢集中的所有電腦定址，而且它會針對每部主機維護一組唯一的固定 IP 位址。 針對負載 \- 平衡的應用程式，當主機失敗或離線時，會在仍在運作的電腦之間自動重新分配負載。 準備好時，離線電腦可以通透方式重新加入叢集，並重新取得它的工作量共用，這能讓叢集中的其他電腦處理較少的流量。

## <a name="practical-applications"></a>實際應用
NLB 適用于確保無狀態應用程式（例如執行 Internet Information Services IIS 的 web \( 伺服器 \) ）可在最短的停機時間內使用，而且可以在 \( 負載增加時新增額外的伺服器來調整規模 \) 。 下列各節說明 NLB 如何支援執行這些應用程式的叢集伺服器之高可用性、延展性和管理性。

### <a name="high-availability"></a>高可用性
高可用性的系統能可靠地以最短的停機時間提供可接受的服務等級。 為了提供高可用性，NLB 包含 \- 可自動執行的內建功能：

-   偵測失敗或離線的叢集主機，然後復原。

-   新增或移除主機時平衡網路負載。

-   10 秒內復原並重新分散工作負載。

### <a name="scalability"></a>延展性
延展性是測量電腦、服務或應用程式可以擴充至何種程度以符合漸增之效能需求的一種方式。 對於 NLB 叢集而言，延展性是指叢集的整體負載超過其功能時，可持續為現有叢集新增一或多個系統的能力。 若要支援延展性，您可以使用 NLB 來執行下列動作：

-   針對個別的 TCP IP 服務，在 NLB 叢集之間平衡負載要求 \/ 。

-   在單一叢集中最多支援 32 部電腦。

-   在叢集中的多部 \( 主機之間，從相同的用戶端或多個用戶端平衡多個伺服器載入要求 \) 。

-   當負載增加時將主機新增至 NLB 叢集，而不會造成叢集失敗。

-   當負載降低時，從叢集移除主機。

-   透過完整管線實作啟用高效能與低負載。 管線可將要求傳送到 NLB 叢集，而不必等候前一個要求的回應。

### <a name="manageability"></a>管理能力
您可以使用 NLB 來執行下列動作以支援管理性：

-   使用 NLB 管理員或網路負載平衡，從單一電腦管理和設定多個 NLB 叢集和叢集主機， [Windows PowerShell 中 (nlb) Cmdlet](/previous-versions/windows/powershell-scripting/hh801274(v=wps.630))。

-   使用連接埠管理規則來指定單一 IP 連接埠或連接埠群組的負載平衡行為。

-   為每個網站定義不同的連接埠規則。 如果您 \- 針對多個應用程式或網站使用同一組負載平衡伺服器，連接埠規則會以 \( 使用虛擬叢集的目的地虛擬 IP 位址為基礎 \) 。

-   使用選擇性的單一主機規則，將所有用戶端要求導向至單一主機 \- 。 NLB 會將用戶端要求路由傳送到執行特定應用程式的特定主機。

-   封鎖特定 IP 連接埠不需要的網路存取權。

-   啟用叢集主機上的網際網路群組管理通訊協定 \( IGMP \) 支援，以控制切換埠氾濫， \( 其中傳入的網路封包在 \) 以多播模式運作時，會傳送到交換器上的所有埠。

-   使用 Windows PowerShell 命令或指令碼，遠端啟動、停止和控制 NLB 動作。

-   檢視 Windows 事件記錄檔來檢查 NLB 事件。 NLB 會在事件記錄檔中記錄所有動作與叢集變更。

## <a name="important-functionality"></a>重要功能

NLB 會安裝為標準的 Windows Server 網路驅動程式元件。 其作業對 TCP IP 網路堆疊而言是透明的 \/ 。 下圖顯示一般設定中的 NLB 與其他軟體元件之間的關聯性。

![網路負載平衡和其他軟體元件](../media/NLB/nlb.jpg)

以下是 NLB 的主要功能。

- 不需要硬體變更就可以執行。

- 提供網路負載平衡工具可讓您自單一遠端或本機電腦設定和管理多個叢集與所有主機。

- 可讓用戶端使用單一的邏輯網際網路名稱和虛擬 IP 位址（稱為叢集 IP 位址）來存取叢集， \( 其會為每部電腦保留個別名稱 \) 。 NLB 允許多重主目錄伺服器使用多個虛擬 IP 位址。

> [!NOTE]
> 當您將 Vm 部署為虛擬叢集時，NLB 不需要將伺服器設為多重主目錄，就能有多個虛擬 IP 位址。

- NLB 可繫結到多個網路介面卡，讓您在每部主機上設定多個獨立叢集。 支援多個網路介面卡與虛擬叢集的不同之處，在於虛擬叢集可讓您在單一網路介面卡上設定多個叢集。

- 不需要對伺服器應用程式進行修改，它們就可以在 NLB 叢集中執行。

- 如果叢集主機失效並且之後恢復上線，可以設定成將主機自動新增到叢集。 新增的主機可以開始處理來自用戶端的新伺服器要求。

-   可讓您使電腦離線以進行預防性維護，而不干擾其他主機上的叢集操作。

## <a name="hardware-requirements"></a>硬體需求
以下是執行 NLB 叢集的硬體需求。

-   在叢集中的所有主機都必須在相同的子網路上。

-   在每部主機上的網路介面卡數目並沒有限制，且不同主機的介面卡數目可以不同。

-   每個叢集內的所有網路介面卡必須都是多點傳送或或都是單點傳播。 NLB 不支援單一叢集內混合使用多點傳送與單點傳播的環境。

-   如果您使用單播模式，用來處理用戶端對叢集流量的網路 \- 配接器 \- 必須支援變更其媒體存取控制 \( MAC \) 位址。

## <a name="software-requirements"></a>軟體需求
以下是執行 NLB 叢集的軟體需求。

-   只有在 \/ 每個主機上啟用 NLB 的介面卡上，才可使用 TCP IP。 請勿將任何其他通訊協定 \( （例如 IPX）新增 \) 至此介面卡。

-   叢集中伺服器的 IP 位址必須是靜態的。

> [!NOTE]
> NLB 不支援動態主機設定通訊協定 \( DHCP \) 。 NLB 會停用它所設定的每個介面上的 DHCP。

## <a name="installation-information"></a>安裝資訊
您可以使用伺服器管理員或 NLB 的 Windows PowerShell 命令來安裝 NLB。

您也可以選擇性地安裝網路負載平衡工具來管理本機或遠端 NLB 叢集。 這些工具組括網路負載平衡管理員和 NLB Windows PowerShell 命令。

### <a name="installation-with-server-manager"></a>使用伺服器管理員安裝

在伺服器管理員中，您可以使用 [新增角色及功能] 嚮導來新增 **網路負載平衡** 功能。 當您完成嚮導時，系統會安裝 NLB，而您不需要重新開機電腦。


### <a name="installation-with-windows-powershell"></a>使用 Windows PowerShell 安裝

若要使用 Windows PowerShell 安裝 NLB，請在您想要安裝 NLB 的電腦上，于提升許可權的 Windows PowerShell 提示字元中執行下列命令。

```powershell
Install-WindowsFeature NLB -IncludeManagementTools
```

安裝完成之後，就不需要重新開機電腦。

如需詳細資訊，請參閱 [Install-WindowsFeature](/powershell/module/servermanager/install-windowsfeature)。

### <a name="network-load-balancing-manager"></a>網路負載平衡管理員
若要在伺服器管理員開啟網路負載平衡管理員，請按一下 [工具]，然後按一下 [網路負載平衡管理員]。

## <a name="additional-resources"></a>其他資源
下表提供 NLB 功能的其他資訊連結。

|內容類型|參考|
|----------------|--------------|
|部署|[網路負載平衡部署指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754833(v=ws.10)) &#124; [使用終端機服務設定網路負載平衡](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771300(v=ws.10))|
|作業|[管理網路負載平衡](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753954(v=ws.10)) 叢集 &#124; [設定網路負載平衡參數](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731619(v=ws.10)) &#124; [控制網路負載平衡叢集上的主機](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770870(v=ws.10))|
|疑難排解|針對[網路負載平衡](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732592(v=ws.10))叢集 &#124; [NLB 叢集事件和錯誤](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731678(v=ws.10))進行疑難排解|
|工具及設定|[網路負載平衡 Windows PowerShell Cmdlet](https://go.microsoft.com/fwlink/p/?LinkId=238123)|
|社群資源|[高可用性 \( 群集 \) 論壇](https://go.microsoft.com/fwlink/p/?LinkId=230641)

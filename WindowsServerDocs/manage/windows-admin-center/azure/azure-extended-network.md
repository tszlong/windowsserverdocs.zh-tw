---
title: 使用 Azure 擴充網路將您的內部部署子網延伸至 Azure
description: 使用 Azure 擴充網路將您的內部部署子網延伸至 Azure
ms.technology: manage
ms.topic: article
author: grcusanz
ms.author: grcusanz
ms.date: 12/17/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 29e370da31b02a475f0fdd5d7914348189ca616b
ms.sourcegitcommit: 6a245fefdf958bfc0aeb69f7a887d11a07bdcd23
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2020
ms.locfileid: "94570353"
---
# <a name="extend-your-on-premises-subnets-into-azure-using-azure-extended-network"></a>使用 Azure 擴充網路將您的內部部署子網延伸至 Azure

> 適用於：Windows Admin Center、Windows Admin Center 預覽版

## <a name="overview"></a>概觀

Azure 擴充的網路可讓您將內部部署子網延展到 Azure，讓內部部署虛擬機器在遷移至 Azure 時保留其原始的內部部署私人 IP 位址。

您可以使用兩個 Windows Server 2019 Vm 之間的雙向 VXLAN 通道（在內部部署執行的虛擬裝置，一個在 Azure 中執行，另一個則是在 Azure 中執行）來擴充網路，每個 Vm 也連線到要擴充的子網。
您即將擴充的每個子網都需要一對設備。 您可以使用多個配對來擴充多個子網。

> [!NOTE]
> Azure 擴充網路只應用於在遷移至 Azure 時無法變更其 IP 位址的機器。 最好是變更 IP 位址，並將它連線到 Azure 中完全存在的子網（如果這是一個選項）。

## <a name="planning"></a>規劃

若要準備使用 Azure 擴充網路，您必須識別要延展的子網，然後執行下列步驟：

### <a name="capacity-planning"></a>容量規劃

您可以使用 Azure 擴充網路延伸最多250個 IP 位址。 您可以預期大約 700 Mbps 的匯總輸送量，並根據 Azure 擴充網路虛擬裝置的 CPU 速度而有一些變化。

### <a name="configuration-in-azure"></a>Azure 中的設定

使用 Windows Admin Center 之前，您必須透過 Azure 入口網站執行下列步驟：

1. 除了閘道連線所需的子網以外，在 Azure 中建立至少包含兩個子網的虛擬網路。 您所建立的其中一個子網必須使用與您想要擴充的內部部署子網相同的子網 CIDR。 子網在您的路由網域內必須是唯一的，因此它不會與任何內部部署子網重迭。

2. 將虛擬網路閘道設定為使用站對站或 ExpressRoute 連線，將虛擬網路連線到您的內部部署網路。

3. 在 Azure 中建立能夠執行嵌套虛擬化的 Windows Server 2019 VM。 這是您的兩個虛擬裝置之一。 將主要網路介面連線到可路由的子網，並將第二個網路介面連線到擴充子網。

4. 啟動 VM、啟用 Hyper-v 角色，然後重新開機。 例如：

    ```powershell
    Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart
    ```

5. 在 VM 中建立兩個外部虛擬交換器，並將其中一個連線到每個網路介面。 例如：

    ```powershell
    New-VMSwitch -Name "External" -AllowManagementOS $true -NetAdapterName "Ethernet"
    New-VMSwitch -Name "Extended" -AllowManagementOS $true -NetAdapterName "Ethernet 2"
    ```

### <a name="on-premises-configuration"></a>內部部署設定

您也必須在內部部署基礎結構中執行一些手動設定，包括建立 VM 作為內部部署虛擬裝置：

1. 請確定子網可在您將部署內部部署 VM (虛擬裝置) 的實體電腦上使用。 這包括您想要擴充的子網，以及第二個子網（唯一且不會與 Azure 虛擬網路中的任何子網重迭）。

2. 在支援嵌套虛擬化的任何虛擬程式上建立 Windows Server 2019 VM。 這是內部部署虛擬裝置。 建議您在叢集中將其建立為高可用性的 VM。 將虛擬網路介面卡連線到可路由傳送的子網，以及將第二個虛擬網路介面卡連接到擴充子網。

3. 啟動 VM，然後從 VM 中的 PowerShell 會話執行此命令以啟用 Hyper-v 角色，然後重新開機 VM：

    ```powershell
    Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart
    ```

4. 在 VM 的 PowerShell 會話中執行下列命令，以在 VM 中建立兩個外部虛擬交換器，並將一個虛擬交換器連線到每個網路介面：

    ```powershell
    New-VMSwitch -Name "External" -AllowManagementOS $true -NetAdapterName "Ethernet"
    New-VMSwitch -Name "Extended" -AllowManagementOS $true -NetAdapterName "Ethernet 2"
    ```

### <a name="additional-prerequisites"></a>其他先決條件

如果您的內部部署網路與 Azure 之間有防火牆，您必須將它設定為允許非對稱式路由。 這可能包括停用序號隨機和啟用 TCP 狀態略過的步驟。 請參閱您的防火牆廠商檔，以瞭解這些步驟。

## <a name="deploy"></a>部署

部署是透過 Windows Admin Center 驅動。

### <a name="install-and-configure-windows-admin-center"></a>安裝和設定 Windows Admin Center

1. [下載並安裝 Windows Admin Center](../understand/windows-admin-center.md) 到能夠執行 Windows Admin Center 的任何電腦上，而不是您稍早建立的兩個虛擬裝置。

2. 在 Windows Admin Center 中，從頁面右上角選取 [ **設定** ] () > **延伸** 模組]。 然後選取 [ **延伸** 模組：

    ![顯示 [設定] 的 [擴充功能] 索引標籤的螢幕擷取畫面](../media/azure-extended-network/installed-extensions.png)

3. 在 [ **可用的擴充** 功能] 索引標籤上，選取 [ **Azure 擴充網路** ]，然後選取 [ **安裝** ]。

    幾秒鐘後，您應該會看到一則訊息，表示安裝成功。

4. [將 Windows Admin Center 連線到 Azure](azure-integration.md)（如果您尚未這樣做）。 如果您現在略過此步驟，我們會要求您稍後在此程式中執行此動作。

5. 在 Windows Admin Center 中，移至 [ **所有連接** ]，  >  **Add** 然後選取 [ **Windows Server** ] 磚中的 [ **新增** ]。 如果內部部署虛擬裝置需要) ，請輸入伺服器名稱 (和認證。

    ![Windows Admin Center 的螢幕擷取畫面，顯示內部部署虛擬裝置上伺服器管理員中的 Azure 擴充網路工具](../media/azure-extended-network/azure-extended-network.png).

6. 按一下 **Azure 擴充網路** 以開始。 您第一次會看到 [總覽] 和 [設定] 按鈕：

    ![Image](../media/azure-extended-network/azure-extended-network-intro.png)

### <a name="deploy-azure-extended-network"></a>部署 Azure 擴充網路

1. 按一下 [設定] 開始 **設定** 。

2. 按一下 **[下一步]** 繼續進行總覽。

3. 在 [上 **傳套件** ] 面板上，您必須下載 Azure 擴充網路代理程式套件，並將它上傳至虛擬裝置。 遵循面板上的指示進行。

    > [!IMPORTANT]
    > 視需要向下移動，然後按一下 [ **上傳** ]，再按 **[下一步]： Extended-Network 安裝**

4. 選取您要擴充之內部部署網路的子網 CIDR。 子網的清單會從虛擬裝置讀取。 如果您尚未將虛擬裝置連線到一組正確的子網，您將不會在此清單中看到所需的子網 CIDR。

5. 選取子網 CIDR 之後，請按 **[下一步]** 。

6. 選取您要擴充的訂用帳戶、資源群組和虛擬網路：

    ![Azure 網路](../media/azure-extended-network/azure-network.png)

    系統會自動選取區域 (Azure 位置) 和子網。 選取 [下一步]： Extended-Network 閘道安裝程式以繼續。

7. 您現在將設定虛擬裝置。 內部部署閘道應該會自動填入其資訊：

    ![內部部署網路閘道](../media/azure-extended-network/on-premises-network-gateway.png)

    如果看起來正確，您可以按 **[下一步]** 。

8. 針對 Azure 虛擬裝置，您必須選取要使用的資源群組和 VM：

    ![Azure 網路閘道](../media/azure-extended-network/azure-network-gateway.png)

9. 選取 VM 之後，您也必須選取 Azure Extended-Network 閘道子網 CIDR。 然後按一下 **[下一步：部署]** 。

10. 查看摘要資訊，然後按一下 [ **部署** ] 開始部署程式。 部署需要大約5-10 分鐘的時間。 部署完成時，您會看到下列用來管理擴充 IP 位址的面板，且狀態應為 **[確定]** ：

    ![安裝完成](../media/azure-extended-network/installation-complete.png)

## <a name="manage"></a>管理

每個您想要在擴充網路間連線的 IP 位址都必須設定。 您最多可以設定250個位址來延長。
若要擴充位址

1. 按一下 [ **新增 IPv4 位址** ：

    ![新增 ipv4 位址](../media/azure-extended-network/add-ipv4-addresses.png)

2. 您會在右側看到 [ **新增 IPv4 位址** ] 飛出視窗：

    ![新增 ipv4 位址面板](../media/azure-extended-network/add-ipv4-addresses-panel.png)

3. 使用 [ **新增** ] 按鈕以手動方式新增位址。 您新增至 Azure 地址清單的 Azure 位址可連線至內部部署的位址，反之亦然。

4. Azure 擴充網路會掃描網路以探索 IP 位址，並根據此掃描填入建議清單。 若要擴充這些位址，您必須使用下拉式清單，並選取探索到的位址旁的核取方塊。 並非所有位址都會被探索到。 （選擇性）使用 [ **新增** ] 按鈕手動新增未自動探索的位址。

    ![使用資訊新增 ipv4 位址面板](../media/azure-extended-network/add-ipv4-addresses-panel-filled.png)

5. 完成時，請按一下 [ **提交** ]。 當設定完成時，您會看到 **更新** 的狀態變更 **，然後進行** 中，最後再回到 **[確定]** 。

    現在已延長您的位址。 您可以使用 [新增 IPv4 位址] 按鈕隨時新增其他位址。 如果某個 IP 位址不再使用於擴充網路的任一端，請選取它旁邊的核取方塊，然後選取 [移除 IPv4 位址]。

如果您不想再使用 Azure 擴充網路，請按一下 [ **移除 Azure 擴充網路** ] 按鈕。 這會從兩個虛擬裝置卸載代理程式，並移除擴充的 IP 位址。 網路將會停止擴充。 如果您想要再次開始使用擴充網路，則必須在移除之後重新執行安裝程式。

## <a name="troubleshooting"></a>疑難排解

如果您在部署 Azure 擴充網路期間收到錯誤，請使用下列步驟：

1. 確認兩個虛擬裝置都使用 Windows Server 2019。

2. 確認您未在其中一個虛擬裝置上執行 Windows Admin Center。 建議您從內部部署網路執行 Windows Admin Center。

3. 請確定您可以使用從 Windows Admin Center 閘道遠端進入內部部署 VM `enter-pssession` 。

4. 如果 Azure 與內部部署之間有防火牆，請確認它已設定為允許所選埠上的 UDP 流量 (預設的 4789) 。 使用 ctsTraffic 之類的工具來設定接聽程式和寄件者。 確認可在指定的埠上以雙向方式傳送流量。

5. 使用 pktmon 來確認封包如預期般傳送和接收。 在每個虛擬裝置上執行 pktmon：

    ```powershell
    Pktmon start –etw
    ```

6. 執行 Azure 擴充網路設定，然後停止追蹤：

    ```powershell
    Pktmon stop
    Netsh trace convert input=<path to pktmon etl file>
    ```

7. 開啟從每個虛擬裝置產生的文字檔，並在指定的埠上搜尋 UDP 流量 (預設的 4789) 。 如果您看到從內部部署虛擬裝置傳送的流量，但 Azure 虛擬裝置未收到流量，則您需要驗證設備之間的路由和防火牆。 如果您看到流量從內部部署傳送到 Azure，您應該會看到 Azure 虛擬裝置傳送封包以回應。 如果內部部署虛擬裝置從未收到該封包，您需要確認路由是否正確，而且沒有防火牆封鎖之間的流量。

### <a name="diagnosing-the-data-path-after-initial-configuration"></a>在初始設定之後診斷資料路徑

設定 Azure 擴充網路之後，您可能會遇到的其他問題通常是因為防火牆封鎖了流量，或在失敗時超過了 MTU。

1. 確認兩個虛擬裝置皆已啟動且正在執行。

2. 確認 Azure 擴充網路代理程式正在每個虛擬裝置上執行：

    ```powershell
    get-service extnwagent
    ```

3. 如果狀態不在執行中，您可以使用下列程式來啟動它：

    ```powershell
    start-service extnwagent
    ```

4. 使用上述步驟5中所述的 packetmon，而流量會在網路上的兩部 Vm 之間傳送，以驗證虛擬裝置所收到的流量，透過設定的 UDP 埠傳送，然後由其他虛擬裝置接收和轉送。

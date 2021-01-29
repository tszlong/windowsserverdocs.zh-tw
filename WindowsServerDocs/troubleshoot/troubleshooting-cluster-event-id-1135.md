---
title: 針對事件識別碼 1135 的叢集問題進行疑難排解
description: 說明如何針對事件識別碼1135的叢集服務啟動問題進行疑難排解。
ms.date: 05/28/2020
author: Deland-Han
ms.author: delhan
ms.topic: troubleshooting
ms.openlocfilehash: 902ce4f5e30811d54d986c63fb8ca30cd0803aa5
ms.sourcegitcommit: d1815253b47e776fb96a3e91556fd231bef8ee6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2021
ms.locfileid: "99042474"
---
# <a name="troubleshooting-cluster-issue-with-event-id-1135"></a>針對事件識別碼 1135 的叢集問題進行疑難排解

本文可協助您診斷和解決事件識別碼1135，這可能是在容錯移轉叢集環境中的叢集服務啟動期間記錄。

## <a name="start-page"></a>起始頁

事件識別碼1135表示已從使用中的容錯移轉叢集成員資格移除一或多個叢集節點。 它可能會伴隨下列徵兆：

- 正在從主動容錯移轉叢集成員資格中移除叢集 Failover\nodes： [從作用中的容錯移轉叢集成員資格移除節點有問題](/problem-nodes-failover-cluster.md)
- 事件識別碼 1069 [事件識別碼 1069-叢集服務或應用程式可用性](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc756225(v=ws.10))
- 仲裁遺失事件識別碼1177的事件識別碼 1177 [—仲裁和仲裁所需的連線能力](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc773498(v=ws.10))
- 叢集服務已停止的事件識別碼1006： [事件識別碼 1006-叢集服務啟動](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc773418(v=ws.10))

系統會建議驗證和網路測試，做為其中一個初始的疑難排解步驟，以確保沒有可能造成問題的設定問題。

### <a name="check-if-installed-the-recommended-hot-fixes"></a>檢查是否已安裝建議的熱修正

叢集服務是一個重要的軟體元件，可控制容錯移轉叢集操作的所有層面，以及管理叢集設定資料庫。 如果您看到事件識別碼1135，Microsoft 建議您安裝下列 KB 文章中提及的修正程式，並重新啟動叢集的所有節點，然後觀察是否發生問題。

- [Windows Server 2012 R2 的修正程式](https://support.microsoft.com/help/2920151)
- [Windows Server 2012 的修正程式](https://support.microsoft.com/help/2784261)
- [Windows Server 2008 R2 的修正程式](https://support.microsoft.com/help/2545685)

### <a name="check-if-the-cluster-service-running-on-all-the-nodes"></a>檢查是否在所有節點上執行的叢集服務

根據您的 Windows 作業系統來執行下列命令，以驗證叢集服務持續執行且可用。

#### <a name="for-windows-server-2008-r2-cluster"></a>若為 Windows Server 2008 R2 叢集

從提高許可權的 cmd 提示字元執行： **cluster.exe 節點/stat**

#### <a name="for-windows-server-2012-and-windows-server-2012-r2-cluster"></a>若為 Windows Server 2012 \ 和 Windows Server 2012 R2 叢集

執行 PS 命令：叢集 **節點/status**

叢集服務會持續在所有節點上執行，並可供使用嗎？

## <a name="solution-for-cluster-service-is-failing"></a>叢集服務的解決方案失敗

如果叢集服務失敗，請使用本文進行疑難排解： [Windows Server 2008 和2008R2 容錯移轉叢集啟動參數](/archive/blogs/askcore/windows-server-2008-and-2008r2-failover-cluster-startup-switches)。


## <a name="several-scenarios-of-event-id-1135"></a>事件識別碼1135的數個案例

我們希望您深入瞭解叢集所有節點上的系統事件記錄檔。 檢查您在節點上看到的事件識別碼1135，並複製此事件的所有實例。 這可讓您輕鬆查看並進行審查。

```console
Event ID 1135
Cluster node ' **NODE A** ' was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

一般案例有三種：

### <a name="scenario-a"></a>案例 A

您正在查看所有事件，且叢集中的所有節點都表示節點 A 已遺失通訊。

![顯示節點 A、節點 B 和節點 C 成功通訊的圖表。 ](media/troubleshooting-cluster-event-id-1135/18647.png)
 ![顯示節點 A 與節點 B 和節點 C 的通訊中斷的圖表。](media/troubleshooting-cluster-event-id-1135/18648.png)

當您看到節點 A 上的系統記錄時，它會有叢集中所有其餘節點的事件。

#### <a name="solution"></a>解決方案

這相當建議在問題發生時，可能是因為網路擁塞或其他節點 A 的通訊遺失。

您應該檢查並驗證網路設定和通訊問題。 請記得尋找與 Node A 相關的問題。

### <a name="scenario-b"></a>案例 B

您正在查看節點上的事件，並讓我們假設您的叢集分散在兩個網站上。 節點 A、節點 B 和節點 C，位於網站1和節點 D & 節點 E （位於網站2）。

![此圖顯示網站1透過 WAN 連結成功與網站2進行通訊。](media/troubleshooting-cluster-event-id-1135/18649.png)

在節點 A、B 和 C 上，您會看到記錄的事件是連接到節點 D & E。同樣地，當您看到節點 D & E 的事件時，這些事件會建議我們遺失與 A、B 和 C 的通訊。

![此圖顯示網站1已遺失與網站2的 WAN 連結連線。](media/troubleshooting-cluster-event-id-1135/18650.png)

#### <a name="solution"></a>解決方案

如果您看到類似的活動，表示在連接這些網站的連結上發生通訊失敗。 建議您複習整個網站的連線，如果這是透過 WAN 連線，建議您向您的 ISP 確認連線能力。

### <a name="scenario-c"></a>案例 C

您正在查看節點上的事件，而且您看到節點的名稱不會以任何特定模式來計算。 讓我們假設您的叢集分散在兩個網站上。 節點 A、節點 B 和節點 C，位於網站1和節點 D & 節點 E （位於網站2）。

- 在節點 A 上：您會看到節點 B、D、E 的事件。
- 在節點 B 上：您會看到節點 C、D、E 的事件。
- 在節點 C 上：您會看到節點 A、B、E 的事件。
- 在節點 D 上：您會看到節點 A、C、E 的事件。
- 節點 E：您會看到節點 B、C、D 的事件。
- 或任何其他組合。

![案例 C](media/troubleshooting-cluster-event-id-1135/18651.png)

#### <a name="solution"></a>解決方案

當節點之間的網路通道 choked，且叢集通訊訊息未及時抵達時，就可能發生這類事件，讓叢集覺得節點之間的通訊會遺失，而導致從叢集成員資格中移除節點。

## <a name="review-cluster-networks"></a>檢查叢集網路

建議您檢查叢集網路，方法是逐一檢查下列三個選項，以繼續進行此疑難排解指南。

### <a name="check-for-antivirus-exclusion"></a>檢查防毒軟體排除

從執行叢集服務的伺服器上的病毒掃描中排除下列檔案系統位置：

- 檔案共用見證的路徑

- *%Systemroot%\Cluster 資料夾*

設定防毒軟體內的即時掃描元件，以排除下列目錄和檔案：

- 預設虛擬機器設定目錄 (C:\ProgramData\Microsoft\Windows\Hyper-V) 

- 自訂虛擬機器設定目錄

- 預設虛擬硬碟目錄 (C:\Users\Public\Documents\Hyper-V\Virtual 硬碟) 

- 自訂虛擬硬碟磁片磁碟機目錄

- 自訂複寫資料目錄（如果您使用 Hyper-v 複本）

- 快照集目錄

- mms.exe

    > [!NOTE]
    > 此檔案可能必須設定為防毒軟體內的進程排除。 ) 

- Vmwp.exe

    > [!NOTE]
    > 此檔案可能必須設定為防毒軟體內的進程排除。

此外，當您同時使用即時移轉和叢集共用磁片區時，請排除 CSV 路徑 *C:\Clusterstorage* 及其所有子目錄。 如果您要針對容錯移轉問題進行疑難排解，或已安裝叢集服務與防毒軟體的一般問題，請暫時卸載防毒軟體，或洽詢軟體的製造商，以判斷防毒軟體是否可與叢集服務搭配使用。 在大部分的情況下，停用防毒軟體是不夠的。 即使您停用防毒軟體，當您重新開機電腦時，仍會載入篩選器驅動程式。

### <a name="check-for-network-port-configuration-in-firewall"></a>檢查防火牆中的網路埠設定

叢集服務會控制伺服器叢集操作及管理叢集資料庫。 叢集是獨立電腦的集合，可作為單一電腦。 管理員、程式設計人員和使用者將叢集視為單一系統。 軟體會在叢集的節點之間分散資料。 如果節點失敗，其他節點會提供先前由遺失節點所提供的服務和資料。 新增或修復節點時，叢集軟體會將某些資料移轉至該節點。

系統服務名稱： **ClusSvc**

|應用程式|通訊協定|連接埠|
|---|---|---|
|叢集服務|UDP|3343|
|叢集服務|TCP|3343 (節點聯結作業期間需要此埠。 ) |
|RPC|TCP|135|
|叢集系統管理員|UDP|137|
|Kerberos|UDP\TCP|464 *|
|SMB|TCP|445|
|隨機配置的高 UDP 埠 * *|UDP|介於1024到65535之間的隨機埠號碼<br/>介於49152到 65535 * * _ 之間的隨機埠號碼|
||||

> [!NOTE]
> 此外，若要在 Windows Server 2008 和更新版本上成功驗證 Windows 容錯移轉叢集，請允許 ICMP4、ICMP6 的輸入和輸出流量。

- 如需詳細資訊，請參閱 [建立 Windows Server 2012 容錯移轉叢集失敗，錯誤 0xc000005e](https://support.microsoft.com/help/2830510)。

- 如需如何自訂這些埠的詳細資訊，請參閱「參考」一節中的「 [Windows 的服務總覽和網路埠需求](https://support.microsoft.com/help/832017/) 」。

這是 Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7、Windows Server 2008 和 Windows Vista 中的範圍。

此外，請執行下列命令來檢查防火牆中的網路埠設定。 例如：此命令可協助判斷用於容錯移轉叢集的埠 3343 available\open：

```console
netsh advfirewall firewall show rule name="Failover Clusters (UDP-In)" verbose
```

### <a name="run-the-cluster-validation-report-for-any-errors-or-warnings"></a>執行叢集驗證報告中的任何錯誤或警告

叢集驗證工具會執行一套測試，以確認您的硬體和設定是否與容錯移轉叢集相容。

遵循下列指示：

1. 執行叢集驗證報告中的任何錯誤或警告。 如需詳細資訊，請參閱 [瞭解叢集驗證測試：網路](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771323(v=ws.11)?redirectedfrom=MSDN)

    ![subhatt1](media/troubleshooting-cluster-event-id-1135/18653.png)

2. 確認網路的警告和錯誤。 如需詳細資訊，請參閱 [瞭解叢集驗證測試：網路](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771323(v=ws.11)?redirectedfrom=MSDN)。

    ![依類別網路分類的結果 ](media/troubleshooting-cluster-event-id-1135/18654.png) ![](media/troubleshooting-cluster-event-id-1135/18655.png)

#### <a name="check-the-list-network-binding-order"></a>檢查清單網路系結順序

這項測試會列出網路系結至每個節點上的介面卡的順序。

[介面卡和系結] 索引標籤會以網路服務存取連線的順序列出連線。 這些連線的順序反映了一般 TCP/IP 呼叫/封包傳送到網路的順序。

遵循下列步驟來變更網路介面卡的系結順序：

1. 依序按一下 _ * [開始]、[ **執行**]、輸入 ncpa.cpl，然後按一下 **[確定]**。 您可以在 [網路連線] 視窗的 [局域 **網路**]**和 [High-Speed 的網際網路**] 區段中看到可用的連接。

2. 在 [ **advanced** ] 功能表上，按一下 [ **advanced Settings**]，然後按一下 [ **介面卡和** 系結] 索引標籤。

3. **在 [連線] 區域中**，選取您要在清單中往上移動的連接。 使用箭號按鈕來移動連接。 一般而言，與網路交談 (網域連線、路由至其他網路等等）的卡片應該是清單) 的第一個系結 (最上層。

叢集節點是多宿主系統。 網路優先順序會影響 DNS 用戶端的輸出網路連線能力。 用於用戶端通訊的網路介面卡應位於系結順序的頂端。 非路由網路可以設定為較低的優先順序。 在 Windows Server 2012 和 Windows Server2012 R2 中，叢集網路驅動程式 ( # A0) 介面卡會自動放在裝訂順序清單的底部。

#### <a name="check-the-validate-network-communication"></a>檢查驗證網路通訊

您網路上的延遲也可能會導致這種情況發生。 這些封包可能不會在節點之間遺失，但在超時時間期限到期之前，這些封包可能無法迅速達到足夠的速度。

這項測試會驗證測試伺服器可以在接受的延遲之內與所有網路通訊。

例如：在 [驗證網路通訊] 底下，您可能會看到網路延遲問題的下列訊息。

```console
Succeeded in pinging network interface node003.contoso.com IP Address 192.168.0.2 from network interface node004.contoso.com IP Address 192.168.0.3 with maximum delay 500 after 1 attempt(s).
Either address 10.0.0.96 is not reachable from 192.168.0.2 or **the ping latency is greater than the maximum allowed 2000 ms** This may be expected, since network interfaces node003.contoso.com - Heartbeat Network and node004.contoso.com - Production Network are on different cluster networks
Either address 192.168.0.2 is not reachable from 10.0.0.96 or **the ping latency is greater than the maximum allowed 2000 ms** This may be expected, since network interfaces node004.contoso.com - Production Network and node003.contoso.com - Heartbeat Network for MSCS are on different cluster networks
```

若為多網站叢集，您可能會增加超時值。 如需詳細資訊，請參閱 [在多網站容錯移轉叢集中設定心跳和 DNS 設定](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197562(v=ws.10)?redirectedfrom=MSDN)。

請向 ISP 確認是否有任何 WAN 連線問題。

檢查是否遇到下列任何問題。

##### <a name="network-packets-lost-between-nodes"></a>節點之間的網路封包遺失

1. 使用效能檢查封包遺失

    如果兩個節點之間的網路上的封包遺失，則這些心跳將會失敗。 我們可以使用效能監視器來查看「已捨棄的網路 Interface\Packets」計數器，以輕鬆找出這是否有問題。 一旦您新增此計數器，請查看平均值、最小值和最大值，如果它們是任何大於零的值，則需要為介面卡調整接收緩衝區。

    ![新增計數器](media/troubleshooting-cluster-event-id-1135/18652.png)

    如果您在 VmWare 虛擬化平臺上遇到網路封包遺失的情況，請參閱「VmWare 虛擬化平臺上安裝的叢集」一節。

2. 升級 NIC 驅動程式

    發生此問題的原因可能是 (IC) \VmTools 或故障的 NIC 介面卡過期的 NIC drivers\Integration 元件。
    如果實體機器上的節點之間遺失網路封包，請使用您的網路介面卡驅動程式更新。 舊的或過時的網路介面卡驅動程式和（或）固件。
    有時，網路卡或交換器的簡單設定不正確，也可能導致遺失心跳。

##### <a name="cluster-installed-in-the-vmware-virtualization-platform"></a>VmWare 虛擬化平臺上安裝的叢集

確認 vmware 環境發生的 VMware 介面卡問題。

如果在高流量高載期間捨棄封包，就可能會發生此問題。 確定沒有發生流量篩選 (例如，使用郵件篩選) 。 在消除這種可能性之後，請逐漸增加客體作業系統中的緩衝區數目並進行驗證。

若要減少高載流量下降，請遵循下列步驟：

1. 使用 Windows 鍵 + R 開啟 [執行] 方塊。
2. 輸入 devmgmt.msc，然後按 **enter**。
3. 展開 [**網路介面卡**]
4. 以滑鼠右鍵按一下 **vmxnet3，然後按一下 [屬性]。**
5. 按一下 [進階]  索引標籤。
6. 按一下 [ **小型 Rx 緩衝區** ] 並增加值。 預設值為512，最大值為8192。
7. 按一下 [ **Rx 環形 #1** 大小]，並增加值。 預設值為1024，最大值為4096。

檢查下列 Url，以確認 VMware 環境發生時的 vmware 介面卡問題：

- [從 VMWARE ESX 上的容錯移轉叢集成員資格中移除的節點？](/archive/blogs/askcore/nodes-being-removed-from-failover-cluster-membership-on-vmware-esx)。

- [ESXi 中 VMXNET3 vNIC 的客體作業系統層級的大型封包遺失](https://kb.vmware.com/s/article/2039495)

##### <a name="noticed-any-network-congestion"></a>已注意到任何網路擁塞

網路擁塞也會造成網路連線問題。

確認您的網路已設定為依據 MS 和廠商建議，請參閱設定 [Windows 容錯移轉叢集網路](/archive/blogs/askcore/configuring-windows-failover-cluster-networks)。

##### <a name="check-the-network-configuration"></a>檢查網路設定

如果仍然無法運作，請檢查您是否已在叢集 GUI 中看到分割的網路，或已在心跳 NIC 上啟用 NIC 小組。

如果您在叢集 GUI 中看到分割的網路，請參閱「分割」的叢集 [網路](/archive/blogs/askcore/partitioned-cluster-networks) 以針對此問題進行疑難排解。

如果您已在「信號」 NIC 上啟用 NIC 小組，請依小組廠商的建議檢查小組軟體功能。

##### <a name="upgrade-the-nic-drivers"></a>升級 NIC 驅動程式

發生此問題的原因可能是已過期的 NIC 驅動程式或 NIC 介面卡發生錯誤。

如果實體機器上的節點之間有網路封包遺失，請更新您的網路介面卡驅動程式。 舊的或過時的網路介面卡驅動程式和（或）固件。

有時，網路卡或交換器的簡單設定不正確，也可能導致遺失心跳。

##### <a name="check-the-network-configuration"></a>檢查網路設定

如果仍然無法運作，請檢查您是否已在叢集 GUI 中看到分割的網路，或已在心跳 NIC 上啟用 NIC 小組。

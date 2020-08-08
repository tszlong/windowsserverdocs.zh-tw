---
title: 使用共用存放裝置的延展式叢集複寫
manager: eldenc
ms.author: nedpyle
ms.topic: get-started-article
author: nedpyle
ms.date: 04/26/2019
ms.assetid: 6c5b9431-ede3-4438-8cf5-a0091a8633b0
ms.openlocfilehash: efc2727c913ac2bab5ea619101ebef12f40a69b2
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87991511"
---
# <a name="stretch-cluster-replication-using-shared-storage"></a>使用共用存放裝置的延展式叢集複寫

>適用於：Windows Server 2019、Windows Server 2016、Windows Server (半年通道)

在這個評估範例中，您將在單一延展式叢集中設定這些電腦及其存放裝置 (其中兩個節點會共用一組存放裝置，以及兩個節點會共用另一組存放裝置)，接著複寫將保留這兩組已在叢集中進行鏡像處理的存放裝置，以便能夠立即進行容錯移轉。 雖然並非必要，但這些節點及其存放裝置應位在不同的實體網站。 有個別的步驟可用來建立 Hyper-V 和檔案伺服器叢集以做為範例案例。

> [!IMPORTANT]
> 在這個評估中，不同網站中的伺服器必須能夠透過網路來與其他伺服器進行通訊，但不會有任何實體連線來連至其他網站的共用存放裝置。 這個案例不會使用儲存空間直接存取。

## <a name="terms"></a>詞彙
本逐步解說使用下列環境做為範例︰

-   名為 **SR-SRV01**、**SR-SRV02**、**SR-SRV03** 及 **SR-SRV04** 的四部伺服器，形成名為 **SR-SRVCLUS** 的單一叢集。

-   代表示兩個不同資料中心的一對邏輯「網站」，一個名為 **Redmond**，而另一個名為 **Bellevue**。

> [!NOTE]
> 您最少可以只使用兩個節點，其中每一個節點分別位於每個網站中。 不過，您將無法只透過兩部伺服器來執行內部網站的容錯移轉。 您最多可以使用 64 個節點。

![此圖顯示 Redmond 中的兩個節點，會使用 Bellevue 網站中相同叢集的兩個節點進行複寫](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_StretchClusterExample.png)

**圖 1：延展式叢集中的存放裝置複寫**

## <a name="prerequisites"></a>必要條件
-   Active Directory 網域服務樹系 (不需要執行 Windows Server 2016)。
-   2-64 部執行 Windows Server 2019 或 Windows Server 2016 Datacenter Edition 的伺服器。 如果您執行的是 Windows Server 2019，如果您確定只複寫最多 2 TB 大小的單一磁片區，就可以改用 Standard Edition。
-   兩組共用的存放裝置，使用 SAS JBOD (例如搭配「儲存空間」)、光纖通道 SAN、共用 VHDX 或 iSCSI 目標。 存放裝置應包含 HDD 和 SSD 媒體的混合，且必須支援「持續保留」。 您要將每組存放裝置設定為只能供其中兩部伺服器使用 (非對稱)。
-   每組存放裝置必須允許建立至少兩個虛擬磁碟，一個供複寫的資料使用，另一個供記錄檔使用。 實體存放裝置的所有資料磁碟上，必須都要有相同的磁區大小。 實體存放裝置的所有記錄檔磁碟上，必須都要有相同的磁區大小。
-   每部伺服器上至少要有一個 1GbE 連線，以進行同步複寫，但最好是 RDMA。
-   每部伺服器至少要有 2 GB 的 RAM 和兩個核心。 如需更多虛擬機器，您需要更多的記憶體和核心。
-   適當的防火牆與路由器規則，以允許在所有節點之間提供 ICMP、SMB (連接埠 445，若為 SMB 直接傳輸，需再加上 5445) 以及 WS-MAN (連接埠 5985) 雙向流量。
-   針對同步複寫，伺服器間的網路頻寬必須足以容納您的 IO 寫入工作負載，以及平均 =5ms 的來回延遲。 非同步複寫沒有建議的延遲值。
-   複寫的儲存體不可位於包含 Windows 作業系統資料夾的磁碟機上。

這其中許多需求都可以使用 `Test-SRTopology` Cmdlet 來判斷。 如果您至少在一部伺服器上安裝儲存體複本或儲存體複本管理工具功能，便可存取此工具。 只要安裝此 Cmdlet 即可，不需要將儲存體複本設定為使用此工具。 下列步驟中會包含詳細資訊。

## <a name="provision-operating-system-features-roles-storage-and-network"></a>佈建作業系統、功能、角色、儲存體及網路

1.  使用 [Server Core] 或 [Server 含桌面體驗] 安裝選項，在所有伺服器節點上安裝 Windows Server。
    > [!IMPORTANT]
    > 從此時開始，一律要以所有伺服器上內建系統管理員群組成員的網域使用者身分登入。 請務必記住，在圖形化伺服器安裝或在 Windows 10 電腦上執行時，一開始要提升 PowerShell 和 CMD 命令提示字元的權限。

2.  新增網路資訊並將節點加入網域，然後將它們重新啟動。
    > [!NOTE]
    > 此時，本指南假設您在延展式叢集中使用了兩對伺服器。 WAN 或 LAN 網路會分隔伺服器，而伺服器會隸屬於實體或邏輯網站。 本指南假設 **SR-SRV01** 和 **SR-SRV02** 位於 Redmond 網站，而 **SR-SRV03** 和 **SR-SRV04** 位於 **Bellevue** 網站。

3.  將第一組共用 JBOD 存放裝置機箱、共用 VHDX、iSCSI 目標或 FC SAN 連接到 **Redmond** 網站中的伺服器。

4.  將第二組存放裝置連接到 **Bellevue** 網站中的伺服器。

5.  視需要在四個節點上全部安裝最新的廠商存放裝置和機箱韌體與驅動程式、最新的廠商 HBA 驅動程式、最新的廠商 BIOS/UEFI 韌體、最新的廠商網路驅動程式，以及最新的主機板晶片組驅動程式。 視需要重新啟動節點。

    > [!NOTE]
    > 如需設定共用儲存體和網路硬體，請參閱硬體廠商的文件。

6.  確定伺服器的 BIOS/UEFI 設定能提供高效能，例如停用 C-State、設定 QPI 速度、啟用 NUMA，以及設定最高的記憶體頻率。 務必將 Windows Server 中的電源管理設定為高效能。 視需要重新啟動。

7.  如下所示設定角色：

    -   **圖形化方法**

        執行 **ServerManager.exe**，然後按一下 [管理]**** 和 [新增伺服器]**** 來新增所有伺服器節點。

        > [!IMPORTANT]
        > 在每個節點上安裝 [容錯移轉叢集]**** 和 [儲存體複本]**** 角色和功能，然後將它們重新啟動。 如果計劃要使用像是 Hyper-V、檔案伺服器等其他角色，您也可以現在安裝它們。

    -   **使用 Windows PowerShell 方法**

        在 **SR-SRV04** 或遠端管理電腦上，於 Windows PowerShell 主控台執行下列命令，為延展式叢集在這四個節點上安裝所需的功能與角色，並將它們重新啟動：

        ```PowerShell
        $Servers = 'SR-SRV01','SR-SRV02','SR-SRV03','SR-SRV04'

        $Servers | foreach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,Failover-Clustering,FS-FileServer -IncludeManagementTools -restart }

        ```

        如需這些步驟的詳細資訊，請參閱[安裝或解除安裝角色、角色服務或功能](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)。


8. 如下所示設定儲存體：

    > [!IMPORTANT]
    > -   您必須在每個機箱上建立兩個磁碟區︰一個供資料使用，而另一個供記錄檔使用。
    > -   記錄檔和資料磁碟必須初始化為 GPT，而非多位元率 (MBR)。
    > -   這兩個資料磁碟區的大小必須相同。
    > -   這兩個記錄檔磁碟區的大小應該相同。
    > -   所有複寫的資料磁碟都必須有相同的磁區大小。
    > -   所有記錄檔磁碟都必須有相同的磁區大小。
    > -   記錄磁碟區應該使用 Flash 型存放裝置和高效能復原設定。 Microsoft 建議記錄檔儲存體應該要比資料儲存體更快。 記錄檔磁碟區不得用於其他工作負載。
    > -   資料磁碟可以使用 HDD、SSD 或階層式組合，而且可以使用鏡像或同位空間，或是 RAID 1 或 10、RAID 5 或 RAID 50。
    > -  記錄磁碟區預設必須至少有 9 GB，而且根據記錄需求，可以更大或更小。
    > - 磁碟區必須使用 NTFS 或 ReFS 格式化。
    > - 檔案伺服器角色只有在操作 Test-SRTopology 時才需要，因為它會開啟必要的防火牆連接埠進行測試。

    -   **對於 JBOD 機箱：**

        1.  確定每組成對的伺服器節點只能看到該網站的存放裝置機箱 (亦即非對稱式存放裝置)，同時已正確設定 SAS 連線。

        2.  遵循[在獨立伺服器上部署儲存空間](../storage-spaces/deploy-standalone-storage-spaces.md)中提供的**步驟 1-3**，使用 Windows PowerShell 或伺服器管理員，使用儲存空間佈建儲存體。

    -   **對於 iSCSI 儲存體：**

        1.  確定每組成對的伺服器節點只能看到該網站的存放裝置機箱 (亦即非對稱式存放裝置)。 如果使用 iSCSI，您應該使用一張以上的網路介面卡。

        2.  使用廠商的文件來佈建儲存體。 如果使用 Windows iSCSI 目標，請參閱 [iSCSI 目標區塊儲存體，作法](../iscsi/iscsi-target-server.md)。

    -   **對於 FC SAN 存放裝置：**

        1.  確定每組成對的伺服器節點只能看到該網站的存放裝置機箱 (亦即非對稱式存放裝置)，而您已將主機正確分區。

        2.  使用廠商的文件來佈建儲存體。

## <a name="configure-a-hyper-v-failover-cluster-or-a-file-server-for-a-general-use-cluster"></a>針對一般用途的叢集設定 Hyper-V 容錯移轉叢集或檔案伺服器

設定伺服器節點之後，下一個步驟就是建立下列其中一種類型的叢集：
*  [Hyper-V 容錯移轉叢集](#BKMK_HyperV)
*  [適用於一般用途叢集的檔案伺服器](#BKMK_FileServer)

### <a name="configure-a-hyper-v-failover-cluster"></a><a name="BKMK_HyperV"></a>設定 Hyper-v 容錯移轉叢集

>[!NOTE]
> 如果您想要建立檔案伺服器叢集，而非 Hyper-V 叢集，請略過本節並移至[設定適用於一般用途叢集的檔案伺服器](#BKMK_FileServer)一節。

您現在將建立標準的容錯移轉叢集。 完成設定、驗證及測試之後，您將會使用儲存體複本加以延展。 您可以直接在叢集節點上，或從包含 Windows Server 遠端伺服器管理工具的遠端系統管理電腦，執行下列所有步驟。

#### <a name="graphical-method"></a>圖形化方法

1. 執行 **cluadmin.msc**。

2. 驗證建議的叢集並分析結果，以確保您能繼續進行。

   > [!NOTE]
   > 由於使用的是非對稱式存放裝置，您應該預期會在叢集驗證時發生存放裝置錯誤。

3. 建立 Hyper-V 計算叢集。 確定叢集名稱等於或少於 15 個字元。 以下使用的範例是 SR-SRVCLUS。 如果節點要位於不同的子網中，您必須為每個子網的叢集名稱建立 IP 位址，並使用「或」相依性。  如需詳細資訊，請參閱設定[多重子網叢集的 IP 位址和相依性–第 III 部分](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698)。

4. 設定檔案共用見證或雲端見證，以在發生網站遺失時提供仲裁。

   > [!NOTE]
   > WIndows Server 現在包含 Cloud (Azure) 型見證的選項。 您可以選擇此仲裁選項，而不是檔案共用見證。

   > [!WARNING]
   > 如需仲裁設定的詳細資訊，請參閱[設定和管理 Windows Server 2012 容錯移轉叢集中的仲裁指南的見證設定](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj612870(v=ws.11))。 如需 `Set-ClusterQuorum` Cmdlet 的詳細資訊，請參閱 [Set-ClusterQuorum](/powershell/module/failoverclusters/set-clusterquorum)。

5. 檢閱 [Windows Server 2012 中 Hyper-V 叢集的網路建議](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn550728(v=ws.11))，並確定您是以最佳方式設定叢集網路。

6. 在 Redmond 網站中，將一個磁碟新增到叢集 CSV。 若要這樣做，在 [存放裝置]**** 區段的 [磁碟]**** 節點中，使用滑鼠右鍵按一下來源磁碟，然後按一下 [新增至叢集共用磁碟區]****。

7. 使用[部署 Hyper-V 叢集](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj863389(v=ws.11))指南，在 **Redmond** 網站中只遵循步驟 7-10 來建立測試虛擬機器，以確保叢集可在第一個測試網站中共用存放裝置的兩個節點內運作正常。

8. 如果您要建立兩個節點延展叢集，必須先新增所有儲存體才能繼續進行。 若要這樣做，在叢集節點上使用系統管理權限開啟 PowerShell 工作階段，並執行下列命令︰`Get-ClusterAvailableDisk -All | Add-ClusterDisk`。

   這是 Windows Server 2016 中內建的行為。

9. 啟動 Windows PowerShell，然後使用 `Test-SRTopology` Cmdlet 來判斷您是否符合所有儲存體複本需求。

    例如，若要驗證兩個建議的延展式叢集節點，其中每一個都有 **D:** 與 **E:** 磁碟區，並且執行測試 30 分鐘：
   1. 將所有可用的存放裝置移至 **SR-SRV01**。
   2. 在容錯移轉叢集管理員的 [角色]**** 區段中，按一下 [建立空白角色]****。
   3. 將線上存放裝置新增至名為 [新角色]**** 的空白角色。
   4. 將所有可用的存放裝置移至 **SR-SRV03**。
   5. 在容錯移轉叢集管理員的 [角色]**** 區段中，按一下 [建立空白角色]****。
   6. 將空白的 [新角色 (2)]**** 移至 **SR-SRV03**。
   7. 將線上存放裝置新增至名為 [新角色 (2)]**** 的空白角色。
   8. 現在您已利用磁碟機代號掛接了所有存放裝置，接著可以使用 `Test-SRTopology` 來評估叢集。

        例如：

        ```
        MD c:\temp

        Test-SRTopology -SourceComputerName SR-SRV01 -SourceVolumeName D: -SourceLogVolumeName E: -DestinationComputerName SR-SRV03 -DestinationVolumeName D: -DestinationLogVolumeName E: -DurationInMinutes 30 -ResultPath c:\temp
        ```

      > [!IMPORTANT]
      > 在指定的來源磁碟區上，若評估期間使用的測試伺服器沒有任何寫入 IO 負載，請考慮新增工作負載，否則 Test-SRTopology 將不會產生有用的報告。 您應該使用和實際執行類似的工作負載來測試，才能看出實際的數字與建議的記錄檔大小。 或者，只要在測試期間，將一些檔案複製到來源磁碟區，或下載並執行 DISKSPD 以產生寫入 IO 即可。 例如，在 D：磁片區中有10分鐘的寫入 IO 工作負載較低的範例：`Diskspd.exe -c1g -d600 -W5 -C5 -b4k -t2 -o2 -r -w5 -i100 d:\test.dat`

1.  檢查 **TestSrTopologyReport-< 日期 >.html** 報告，以確定您符合儲存體複本需求，並記下初始同步時間預測和記錄檔建議。

      ![此畫面顯示複寫報告](./media/Stretch-Cluster-Replication-Using-Shared-Storage/SRTestSRTopologyReport.png)

2.  將磁碟回復成可用的存放裝置，並移除暫時的空白角色。

3.  一旦滿足之後，請移除測試虛擬機器。 視需要將任何實際的測試虛擬機器新增到建議的來源節點，以進行進一步評估。

4.  設定延展式叢集網站感知，如此一來，伺服器 **SR-SRV01** 和 **SR-SRV02** 會位於 **Redmond** 網站，且 **SR-SRV03** 和 **SR-SRV04** 會位於 **Bellevue** 網站，而 **Redmond** 是適用於來源存放裝置和 VM 之節點擁有權的慣用網站：

    ```PowerShell
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"

    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"

    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue

    (Get-Cluster).PreferredSite="Seattle"
    ```

    > [!NOTE]
    > 沒有任何選項可以使用 Windows Server 2016 中的容錯移轉叢集管理員來設定網站感知。

5.  ** (選擇性) **設定叢集網路和 Active Directory，以取得更快的 DNS 網站容錯移轉。 您可以利用 Hyper-V 軟體定義的網路功能、延展的 VLAN、網路抽象裝置、降低的 DNS TTL，以及其他常見的技巧。

    如需詳細資訊，請參閱 Microsoft Ignite 工作階段：[Stretching Failover Clusters and Using Storage Replica in Windows Server vNext](https://channel9.msdn.com/Events/Ignite/2015/BRK3487) (在 Windows Server vNext 中延展容錯移轉叢集和使用儲存體複本) 和 [Enable Change Notifications between Sites - How and Why?](/archive/blogs/qzaidi/enable-change-notifications-between-sites-how-and-why) (啟用網站之間的變更通知 - 方式與原因？) 部落格文章。

6.  **(選擇性)** 設定 VM 復原能力，讓客體在節點失敗期間不會長時間暫停。 相反地，在它們會在 10 秒內容錯移轉至新的複寫來源存放裝置。

    ```PowerShell
    (Get-Cluster).ResiliencyDefaultPeriod=10
    ```

    > [!NOTE]
    >  Windows Server 2016 中的容錯移轉叢集管理員，沒有可供設定 VM 復原能力的選項。

#### <a name="windows-powershell-method"></a>Windows PowerShell 方法

1. 測試建議的叢集並分析結果，以確保您能繼續進行：

   ```PowerShell
   Test-Cluster SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04
   ```

   > [!NOTE]
   >  由於使用的是非對稱式存放裝置，您應該預期會在叢集驗證時發生存放裝置錯誤。

2. 建立一般使用儲存叢集的檔案伺服器 (您必須指定您自己的靜態 IP 位址，叢集將會使用) 。 確定叢集名稱等於或少於 15 個字元。  如果節點位於不同的子網中，則必須使用「或」相依性來建立其他網站的 IP 位址。 如需詳細資訊，請參閱設定[多重子網叢集的 IP 位址和相依性–第 III 部分](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698)。
   ```PowerShell
   New-Cluster -Name SR-SRVCLUS -Node SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04 -StaticAddress <your IP here>
   Add-ClusterResource -Name NewIPAddress -ResourceType “IP Address” -Group “Cluster Group”
   Set-ClusterResourceDependency -Resource “Cluster Name” -Dependency “[Cluster IP Address] or [NewIPAddress]”
   ```

3. 在指向共用 (裝載於網域控制站或一些其他獨立伺服器上) 的叢集中，設定檔案共用見證或雲端 (Azure) 見證。 例如：

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\someserver\someshare
   ```

   > [!NOTE]
   > WIndows Server 現在包含 Cloud (Azure) 型見證的選項。 您可以選擇此仲裁選項，而不是檔案共用見證。

   如需仲裁設定的詳細資訊，請參閱[設定和管理 Windows Server 2012 容錯移轉叢集中的仲裁指南的見證設定](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj612870(v=ws.11))。 如需 `Set-ClusterQuorum` Cmdlet 的詳細資訊，請參閱 [Set-ClusterQuorum](/powershell/module/failoverclusters/set-clusterquorum)。

4. 檢閱 [Windows Server 2012 中 Hyper-V 叢集的網路建議](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn550728(v=ws.11))，並確定您是以最佳方式設定叢集網路。

5. 如果您要建立兩個節點延展叢集，必須先新增所有儲存體才能繼續進行。 若要這樣做，在叢集節點上使用系統管理權限開啟 PowerShell 工作階段，並執行下列命令︰`Get-ClusterAvailableDisk -All | Add-ClusterDisk`。

   這是 Windows Server 2016 中內建的行為。

6. 使用[部署 Hyper-V 叢集](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj863389(v=ws.11))指南，在 **Redmond** 網站中只遵循步驟 7-10 來建立測試虛擬機器，以確保叢集可在第一個測試網站中共用存放裝置的兩個節點內運作正常。

7. 一旦滿足之後，請移除測試 VM。 視需要將任何實際的測試虛擬機器新增到建議的來源節點，以進行進一步評估。

8. 設定延展式叢集網站感知，如此一來，伺服器 **SR-SRV01** 和 **SR-SRV02** 會位於 **Redmond** 網站，且 **SR-SRV03** 和 **SR-SRV04** 會位於 **Bellevue** 網站，而 **Redmond** 是適用於來源存放裝置和虛擬機器之節點擁有權的慣用網站：

   ```PowerShell
   New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"

   New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"

   Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle
   Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle
   Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue
   Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue

   (Get-Cluster).PreferredSite="Seattle"
   ```

9. ** (選擇性) **設定叢集網路和 Active Directory，以取得更快的 DNS 網站容錯移轉。 您可以利用 Hyper-V 軟體定義的網路功能、延展的 VLAN、網路抽象裝置、降低的 DNS TTL，以及其他常見的技巧。

   如需詳細資訊，請參閱 Microsoft Ignite 工作階段：[Stretching Failover Clusters and Using Storage Replica in Windows Server vNext](https://channel9.msdn.com/Events/Ignite/2015/BRK3487) (在 Windows Server vNext 中延展容錯移轉叢集和使用儲存體複本) 和 [Enable Change Notifications between Sites - How and Why](/archive/blogs/qzaidi/enable-change-notifications-between-sites-how-and-why) (啟用網站之間的變更通知 - 方式與原因)。

10. **(選擇性)** 設定 VM 復原能力，讓來賓不會因為長期處於節點失敗而暫停。 相反地，在它們會在 10 秒內容錯移轉至新的複寫來源存放裝置。

    ```PowerShell
    (Get-Cluster).ResiliencyDefaultPeriod=10
    ```

    > [!NOTE]
    > Windows Server 2016 中的容錯移轉叢集管理員，沒有 VM 復原能力的選項。



### <a name="configure-a-file-server-for-general-use-cluster"></a><a name="BKMK_FileServer"></a>設定一般用途叢集的檔案伺服器

>[!NOTE]
> 如果您已經設定 Hyper-V 容錯移轉叢集 (如[設定 Hyper-V 容錯移轉叢集](#BKMK_HyperV)中所述)，請略過本節。

您現在將建立標準的容錯移轉叢集。 完成設定、驗證及測試之後，您將會使用儲存體複本加以延展。 您可以直接在叢集節點上，或從包含 Windows Server 遠端伺服器管理工具的遠端系統管理電腦，執行下列所有步驟。

#### <a name="graphical-method"></a>圖形化方法

1. 執行 cluadmin.msc。

2. 驗證建議的叢集並分析結果，以確保您能繼續進行。
   >[!NOTE]
   >由於使用的是非對稱式存放裝置，您應該預期會在叢集驗證時發生存放裝置錯誤。
3. 建立適用於一般用途存放裝置叢集的檔案伺服器。 確定叢集名稱等於或少於 15 個字元。 以下使用的範例是 SR-SRVCLUS。  如果節點要位於不同的子網中，您必須為每個子網的叢集名稱建立 IP 位址，並使用「或」相依性。  如需詳細資訊，請參閱設定[多重子網叢集的 IP 位址和相依性–第 III 部分](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698)。

4. 設定檔案共用見證或雲端見證，以在發生網站遺失時提供仲裁。
   >[!NOTE]
   > WIndows Server 現在包含 Cloud (Azure) 型見證的選項。 您可以選擇此仲裁選項，而不是檔案共用見證。
   >[!NOTE]
   >  如需仲裁設定的詳細資訊，請參閱[設定和管理 Windows Server 2012 容錯移轉叢集中的仲裁指南的見證設定](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj612870(v=ws.11))。 如需 Set-ClusterQuorum Cmdlet 的詳細資訊，請參閱 [Set-ClusterQuorum](/powershell/module/failoverclusters/set-clusterquorum)。

5. 如果您要建立兩個節點延展叢集，必須先新增所有儲存體才能繼續進行。 若要這樣做，在叢集節點上使用系統管理權限開啟 PowerShell 工作階段，並執行下列命令︰`Get-ClusterAvailableDisk -All | Add-ClusterDisk`。

   這是 Windows Server 2016 中內建的行為。

6. 確定您是以最佳方式設定叢集網路。
    >[!NOTE]
    > 繼續進行下一個步驟之前，必須先在所有節點上安裝檔案伺服器角色。   |

7. 在 [角色]**** 中，按一下 [設定角色]****。 檢閱 [在您開始前]****，然後按 [下一步]****。

8. 選取 [檔案伺服器]****，然後按 [下一步]****。

9. 保留選取 [一般用途的檔案伺服器]****，然後按 [下一步]****。

10. 為 [用戶端存取點]**** 命名 (15 個字元或更少)，然後按 [下一步]****。

11. 選取磁碟做為您的資料磁碟區，然後按 [下一步]****。

12. 檢閱您的設定，然後按 [下一步]****。 按一下 [完成] 。

13. 在新的檔案伺服器角色上按一下滑鼠右鍵，然後按一下 [新增檔案共用]****。 繼續執行精靈以設定共用。

14. 選擇性︰新增其他會在此網站中使用另一個存放裝置的檔案伺服器角色。

15. 設定延展式叢集網站感知，如此一來，伺服器 SR-SRV01 和 SR-SRV02 會位於 Redmond 網站，且 SR-SRV03 和 SR-SRV04 會位於 Bellevue 網站，而 Redmond 是適用於來源存放裝置和 VM 之節點擁有權的慣用網站：

    ```PowerShell
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"

    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"

    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue

    (Get-Cluster).PreferredSite="Seattle"
    ```

      >[!NOTE]
      > 沒有任何選項可以使用 Windows Server 2016 中的容錯移轉叢集管理員來設定網站感知。

16. (選擇性) 設定叢集網路和 Active Directory，以進行更快速的 DNS 網站容錯移轉。 您可以利用延展的 VLAN、網路抽象裝置、降低的 DNS TTL，以及其他常見的技巧。

如需詳細資訊，請參閱 Microsoft Ignite 工作階段：[Stretching Failover Clusters and Using Storage Replica in Windows Server vNext](https://channel9.msdn.com/events/ignite/2015/brk3487) (在 Windows Server vNext 中延展容錯移轉叢集和使用儲存體複本) 和部落格文章：[Enable Change Notifications between Sites - How and Why](/archive/blogs/qzaidi/enable-change-notifications-between-sites-how-and-why) (啟用網站之間的變更通知 - 方式與原因)。

#### <a name="powershell-method"></a>PowerShell 方法

1. 測試建議的叢集並分析結果，以確保您能繼續進行：

    ```PowerShell
    Test-Cluster SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04
    ```

    > [!NOTE]
    >  由於使用的是非對稱式存放裝置，您應該預期會在叢集驗證時發生存放裝置錯誤。

2.  建立 Hyper-V 計算叢集 (您必須指定自己的靜態 IP 位址，而叢集將使用此位址)。 確定叢集名稱等於或少於 15 個字元。  如果節點位於不同的子網中，則必須使用「或」相依性來建立其他網站的 IP 位址。 如需詳細資訊，請參閱設定[多重子網叢集的 IP 位址和相依性–第 III 部分](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698)。

    ```PowerShell
    New-Cluster -Name SR-SRVCLUS -Node SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04 -StaticAddress <your IP here>

    Add-ClusterResource -Name NewIPAddress -ResourceType “IP Address” -Group “Cluster Group”

    Set-ClusterResourceDependency -Resource “Cluster Name” -Dependency “[Cluster IP Address] or [NewIPAddress]”
    ```


3. 在指向共用 (裝載於網域控制站或一些其他獨立伺服器上) 的叢集中，設定檔案共用見證或雲端 (Azure) 見證。 例如：

    ```PowerShell
    Set-ClusterQuorum -FileShareWitness \\someserver\someshare
    ```

    >[!NOTE]
    > Windows Server 現在包含使用 Azure 的雲端見證選項。 您可以選擇此仲裁選項，而不是檔案共用見證。

   如需仲裁設定的詳細資訊，請參閱[瞭解叢集和集區仲裁](../storage-spaces/understand-quorum.md)。 如需 Set-ClusterQuorum Cmdlet 的詳細資訊，請參閱 [Set-ClusterQuorum](/powershell/module/failoverclusters/set-clusterquorum)。

4.  如果您要建立兩個節點延展叢集，必須先新增所有儲存體才能繼續進行。 若要這樣做，在叢集節點上使用系統管理權限開啟 PowerShell 工作階段，並執行下列命令︰`Get-ClusterAvailableDisk -All | Add-ClusterDisk`。

    這是 Windows Server 2016 中內建的行為。

5. 確定您是以最佳方式設定叢集網路。

6.  設定檔案伺服器角色。 例如：

    ```PowerShell
    Get-ClusterResource
    Add-ClusterFileServerRole -Name SR-CLU-FS2 -Storage "Cluster Disk 4"

    MD e:\share01

    New-SmbShare -Name Share01 -Path f:\share01 -ContinuouslyAvailable $false
    ```

7. 設定延展式叢集網站感知，如此一來，伺服器 SR-SRV01 和 SR-SRV02 會位於 Redmond 網站，且 SR-SRV03 和 SR-SRV04 會位於 Bellevue 網站，而 Redmond 是適用於來源存放裝置和虛擬機器之節點擁有權的慣用網站：

    ```PowerShell
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"

    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"

    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue

    (Get-Cluster).PreferredSite="Seattle"
    ```

8.  (選擇性) 設定叢集網路和 Active Directory，以進行更快速的 DNS 網站容錯移轉。 您可以利用延展的 VLAN、網路抽象裝置、降低的 DNS TTL，以及其他常見的技巧。

    如需詳細資訊，請參閱 Microsoft Ignite 工作階段：[Stretching Failover Clusters and Using Storage Replica in Windows Server vNext](https://channel9.msdn.com/events/ignite/2015/brk3487) (在 Windows Server vNext 中延展容錯移轉叢集和使用儲存體複本) 和部落格文章：[Enable Change Notifications between Sites - How and Why](/archive/blogs/qzaidi/enable-change-notifications-between-sites-how-and-why) (啟用網站之間的變更通知 - 方式與原因)。

### <a name="configure-a-stretch-cluster"></a>設定延展式叢集
現在您將使用容錯移轉叢集管理員或 Windows PowerShell，來設定延展式叢集。 您可以直接在叢集節點上，或從包含 Windows Server 遠端伺服器管理工具的遠端系統管理電腦，執行下列所有步驟。

#### <a name="failover-cluster-manager-method"></a>容錯移轉叢集管理員方法

1.  對於 Hyper-V 工作負載，在某一個您想要複寫出資料的節點上，從您的可用磁碟將來源資料磁碟新增至叢集共用磁碟區 (如果尚未設定)。 請勿新增所有磁碟；只要新增單一磁碟。 此時，有一半的磁碟將會顯示離線，因為這是非對稱式存放裝置。
如果複寫實體磁碟資源 (PDR) 工作負載類似一般用途的檔案伺服器，您就已經備妥角色連接的磁碟。

    ![此畫面顯示容錯移轉叢集管理員](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_OnlineDisks2.png)

2.  在 CSV 磁碟或角色連接的磁碟上按一下滑鼠右鍵、按一下 [複寫]****，然後按一下 [啟用]****。

3.  選取適當的目的地資料磁碟區，然後按 [下一步]****。 顯示的目的地磁碟將擁有大小與所選取來源磁碟相同的磁碟區。 在這些精靈對話方塊之間移動時，可用的存放裝置將會自動移動，並視需要在背景上線。

    ![此畫面顯示 [設定存放裝置複本] 精靈的 [選取目的地磁碟] 頁面](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_SelectDestinationDataDisk2.png)

4.  選取適當的來源記錄磁碟，然後按 [下一步]****。 來源記錄磁碟區必須是在使用 SSD 或同樣快速媒體的磁碟上，而不是轉盤式磁碟。

5.  選取適當的目的地記錄檔磁碟區，然後按 [下一步]。 顯示的目的地記錄磁碟將擁有大小與所選來源記錄磁碟區相同的磁碟區。

6.  如果目的地磁碟區不包含先前來自來源伺服器的資料複本，請保留 [覆寫目的地磁碟區]**** 上的 [覆寫磁碟區]**** 值。 如果目的地包含來自最新備份或先前複寫的類似資料，選取 [已植入資料的目的地磁碟]****，然後按 [下一步]****。

7.  如果您不打算使用 RPO 複寫，請保留 [同步複寫]**** 上的 [複寫模式]**** 值。 如果您想要透過較高延遲的網路來延展叢集，或在主要網站節點上需要較低的 IO 延遲，請將它變更為 [非同步複寫]****。
8.  如果您不打算稍後搭配複寫群組中的其他磁碟配對來使用寫入順序，請保留 [最高效能]**** 上的 [一致性群組]**** 值。 如果您打算進一步將磁碟新增到此複寫群組，而且您需要保證寫入順序，選取 [啟用寫入順序]****，然後按 [下一步]****。

9.  按 [下一步]**** 以設定複寫和延展性叢集資訊。

    ![此畫面顯示 [設定存放裝置複本] 精靈的 [選取確認] 頁面](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_ConfigureSR2.png)

10. 在 [摘要] 畫面中，記下完成的對話方塊結果。 您可以在 Web 瀏覽器中檢視此報告。

11. 此時，您已經在這兩半的叢集之間設定了儲存體複本關聯性，但複寫正在進行中。 有數種方式可以透過圖形化工具來檢視複寫的狀態。

    1.  使用 **\[複寫角色\]** 欄和 **\[複寫\]** 索引標籤。完成初始同步時，來源和目的地磁碟的複寫狀態都必須是 **\[持續複寫中\]**。

        ![此畫面顯示容錯移轉叢集管理員中磁碟的 [複寫] 索引標籤](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_ReplicationDetails2.png)

    2.  啟動 **eventvwr.exe**。

        1.  在來源伺服器上，瀏覽至 **應用程式和服務 \ Microsoft \ Windows \ StorageReplica \ 系統管理**，然後檢查事件5015、5002、5004、1237、5001 及 2200。

        2.  在目的地伺服器上，瀏覽至 **應用程式和服務 \ Microsoft \ Windows \ StorageReplica \ 操作**，然後等候事件 1215。 此事件會說明已複製的位元組數目和所花費的時間。 範例：

            ```
            Log Name:      Microsoft-Windows-StorageReplica/Operational
            Source:        Microsoft-Windows-StorageReplica
            Date:          4/6/2016 4:52:23 PM
            Event ID:      1215
            Task Category: (1)
            Level:         Information
            Keywords:      (1)
            User:          SYSTEM
            Computer:      SR-SRV03.Threshold.nttest.microsoft.com
            Description:
            Block copy completed for replica.

            ReplicationGroupName: Replication 2
            ReplicationGroupId: {c6683340-0eea-4abc-ab95-c7d0026bc054}
            ReplicaName: \\?\Volume{43a5aa94-317f-47cb-a335-2a5d543ad536}\
            ReplicaId: {00000000-0000-0000-0000-000000000000}
            End LSN in bitmap:
            LogGeneration: {00000000-0000-0000-0000-000000000000}
            LogFileId: 0
            CLSFLsn: 0xFFFFFFFF
            Number of Bytes Recovered: 68583161856
            Elapsed Time (ms): 140
            ```

        3.  在目的地伺服器上，瀏覽至 **應用程式和服務 \ Microsoft \ Windows \ StorageReplica \ 系統管理**，然後檢查事件 5009、1237、5001、5015、5005 及 2200，以了解處理進度。 此序列中應該不會有任何錯誤警告。 其中將會有許多指出進度的 1237 事件。

            > [!WARNING]
            > 在初始同步完成之前，CPU 和記憶體使用量很可能會超過正常情況。

#### <a name="windows-powershell-method"></a>Windows PowerShell 方法

1.  確定您是以提升權限的系統管理員帳戶來執行 Powershell 主控台。
2.  只能將來源資料存放裝置新增至叢集以做為 CSV。 若要取得可用磁碟的大小、磁碟分割及磁碟區配置，請使用下列命令：

    ```PowerShell
    Move-ClusterGroup -Name "available storage" -Node sr-srv01

    $DiskResources = Get-ClusterResource | Where-Object { $_.ResourceType -eq 'Physical Disk' -and $_.State -eq 'Online' }
    $DiskResources | foreach {
        $resource = $_
        $DiskGuidValue = $resource | Get-ClusterParameter DiskIdGuid

        Get-Disk | where { $_.Guid -eq $DiskGuidValue.Value } | Get-Partition | Get-Volume |
            Select @{N="Name"; E={$resource.Name}}, @{N="Status"; E={$resource.State}}, DriveLetter, FileSystemLabel, Size, SizeRemaining
    } | FT -AutoSize

    Move-ClusterGroup -Name "available storage" -Node sr-srv03

    $DiskResources = Get-ClusterResource | Where-Object { $_.ResourceType -eq 'Physical Disk' -and $_.State -eq 'Online' }
    $DiskResources | foreach {
        $resource = $_
        $DiskGuidValue = $resource | Get-ClusterParameter DiskIdGuid

        Get-Disk | where { $_.Guid -eq $DiskGuidValue.Value } | Get-Partition | Get-Volume |
            Select @{N="Name"; E={$resource.Name}}, @{N="Status"; E={$resource.State}}, DriveLetter, FileSystemLabel, Size, SizeRemaining
    } | FT -AutoSize
    ```

4.  使用下列方式，將正確的磁碟設定為 CSV：

    ```PowerShell
    Add-ClusterSharedVolume -Name "Cluster Disk 4"
    Get-ClusterSharedVolume
    Move-ClusterSharedVolume -Name "Cluster Disk 4" -Node sr-srv01
    ```

5.  設定延展式叢集，指定下列內容：

    -   來源和目的地節點 (其中的來源資料是 CSV 磁碟，但所有其他磁碟都不是)。

    -   來源和目的地複寫群組名稱。

    -   來源和目的地磁碟，其中的磁碟分割大小會相符。

    -   來源和目的地記錄磁碟區，其中沒有足夠的可用空間來容納這兩個磁碟上的記錄檔大小，而此存放裝置是 SSD 或同樣快速的媒體。

    -   來源和目的地記錄磁碟區，其中沒有足夠的可用空間來容納這兩個磁碟上的記錄檔大小，而此存放裝置是 SSD 或同樣快速的媒體。

    -   記錄檔大小。

    -   來源記錄磁碟區必須是在使用 SSD 或同樣快速媒體的磁碟上，而不是轉盤式磁碟。

    ```PowerShell
    New-SRPartnership -SourceComputerName sr-srv01 -SourceRGName rg01 -SourceVolumeName "C:\ClusterStorage\Volume1" -SourceLogVolumeName e: -DestinationComputerName sr-srv03 -DestinationRGName rg02 -DestinationVolumeName d: -DestinationLogVolumeName e:
    ```

    > [!NOTE]
    > 您也可以在每個網站的某一個節點上使用 `New-SRGroup` 和 `New-SRPartnership`，分階段建立複寫，而不是一次建立全部。

6.  判斷複寫進度。

    1.  在來源伺服器上，執行下列命令，並檢查 5015、5002、5004、1237、5001 及 2200 事件︰

        ```PowerShell
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20
        ```

    2.  在目的地伺服器上，執行下列命令來查看可顯示建立合作關係的儲存體複本事件。 此事件會說明已複製的位元組數目和所花費的時間。 範例：

        ```
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | fl

        TimeCreated  : 4/6/2016 4:52:23 PM
        ProviderName : Microsoft-Windows-StorageReplica
        Id           : 1215
        Message      : Block copy completed for replica.

               ReplicationGroupName: Replication 2
               ReplicationGroupId: {c6683340-0eea-4abc-ab95-c7d0026bc054}
               ReplicaName: ?Volume{43a5aa94-317f-47cb-a335-2a5d543ad536}
               ReplicaId: {00000000-0000-0000-0000-000000000000}
               End LSN in bitmap:
               LogGeneration: {00000000-0000-0000-0000-000000000000}
               LogFileId: 0
               CLSFLsn: 0xFFFFFFFF
               Number of Bytes Recovered: 68583161856
               Elapsed Time (ms): 140
        ```

    3.  在目的地伺服器上，執行下列命令，並檢查 5009、1237、5001、5015、5005 及 2200 事件，以了解處理進度。 此序列中應該不會有任何錯誤警告。 其中將會有許多指出進度的 1237 事件。

        ```PowerShell
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL
        ```

    4.  或者，複本的目的地伺服器群組會隨時說明待複製的位元組數目，並可透過 PowerShell 進行查詢。 例如：

        ```PowerShell
        (Get-SRGroup).Replicas | Select-Object numofbytesremaining
        ```

        和進度範例 (將不會終止) 一樣：

        ```PowerShell
        while($true) {

         $v = (Get-SRGroup -Name "Replication 2").replicas | Select-Object numofbytesremaining
         [System.Console]::Write("Number of bytes remaining: {0}`r", $v.numofbytesremaining)
         Start-Sleep -s 5
        }
        ```

7.  若要在延展式叢集內取得複寫來源和目的地狀態，使用 `Get-SRGroup` 和 `Get-SRPartnership` 來查看延展式叢集中複寫的設定狀態。

    ```PowerShell
    Get-SRGroup
    Get-SRPartnership
    (Get-SRGroup).replicas
    ```

### <a name="manage-stretched-cluster-replication"></a>管理延展式叢集複寫
現在您將要管理與操作延展式叢集。 您可以直接在叢集節點上，或從包含 Windows Server 遠端伺服器管理工具的遠端系統管理電腦，執行下列所有步驟。

#### <a name="graphical-tools-method"></a>圖形化工具方法

1.  使用容錯移轉叢集管理員，來判斷目前的複寫來源與目的地及其狀態。

2.  若要測量複寫效能，請在來源和目的地節點上執行 **Perfmon.exe**。

    1.  在目的地節點上：

        1.  針對資料磁碟區新增**儲存體複本統計資料**物件及其效能計數器。

        2.  檢查結果。

    2.  在來源節點上：

        1.  針對資料磁碟區新增**儲存體複本統計資料**和**儲存體複本磁碟分割 I/O 統計資料**物件及其所有的效能計數器 (後者只適用於目前來源伺服器上的資料)。

        2.  檢查結果。

3.  若要改變延展式叢集內的複寫來源和目的地，請使用下列方法：

    1.  若要在相同網站的節點之間移動來源複寫︰在來源 CSV 上按一下滑鼠右鍵、按一下 [移動存放裝置]****、按一下 [選取節點]****，然後選取同一個網站中的節點。 如果針對已指派角色的磁碟使用非 CSV 的存放裝置，您就要移動該角色。

    2.  若要將來源複寫從某一個網站移至另一個網站︰在來源 CSV 上按一下滑鼠右鍵、按一下 [移動存放裝置]****、按一下 [選取節點]****，然後選取另一個網站中的節點。 如果您設定了慣用的網站，就可以使用最可能的節點，一律將來源存放裝置移至慣用網站中的節點。 如果針對已指派角色的磁碟使用非 CSV 的存放裝置，您就要移動該角色。

    3.  若要執行計劃的容錯移轉且複寫方向是從某一個網站到另一個網站：在某一個網站中，使用 **ServerManager.exe** 或 **SConfig** 同時關閉這兩個節點。

    4.  若要執行非計劃的容錯移轉且複寫方向是從某一個網站到另一個網站：在某一個網站中，同時關閉這兩個節點的電源。

        > [!NOTE]
        > 在 Windows Server 2016 中，您可能需要使用容錯移轉叢集管理員或 Move-ClusterGroup，在節點重新上線之後，手動將目的地磁碟移回另一個網站。

        > [!NOTE]
        > 儲存空間複本會卸載目的地磁碟區。 這是原廠設定。

4.  若要變更預設 8 GB 的記錄檔大小，請以滑鼠右鍵按一下來源和目的地記錄磁片，按一下 [複寫**記錄**檔] 索引標籤，然後變更兩個磁片上的大小以符合。

    > [!NOTE]
    > 預設記錄檔大小為 8 GB。 根據 `Test-SRTopology` Cmdlet 的結果，您可能會決定以較高或較低的值來使用 `-LogSizeInBytes`。

5.  若要將另一對複寫的磁碟新增到現有的複寫群組，您必須確定在可用存放裝置中至少有一個額外的磁碟。 您接著可以滑鼠右鍵按一下來源磁碟，並選取 [新增複寫合作關係]****。
    > [!NOTE]
    > 可用的存放裝置中需要「虛設」磁碟，是因為要用於迴歸而非刻意。 容錯移轉叢集管理員先前通常支援新增更多磁碟，這在未來的版本中將會再度支援。


6.  移除現有的複寫：

    1.  啟動 **cluadmin.msc**。

    2.  以滑鼠右鍵按一下來源 CSV 磁碟、按一下 [複寫]****，然後按一下 [移除]****。 接受警告提示。

    3.  (選擇性) 從 CSV 移除存放裝置，並讓它回到可用的存放裝置中，以進行進一步測試。

        > [!NOTE]
        > 在返回可用的存放裝置之後，您可能需要使用 **DiskMgmt.msc** 或 **ServerManager.exe**，將磁碟機代號加回磁碟區。

#### <a name="windows-powershell-method"></a>Windows PowerShell 方法

1.  使用 **Get-SRGroup** 和 **(Get-SRGroup).Replicas**，來判斷目前的複寫來源與目的地及其狀態。

2.  若要測量複寫效能，請在來源和目的地節點上使用 `Get-Counter` Cmdlet。 計數器名稱如下︰

    -   \Storage Replica Partition I/O Statistics(*)\Number of times flush paused

    -   \Storage Replica Partition I/O Statistics(*)\Number of pending flush I/O

    -   \Storage Replica Partition I/O Statistics(*)\Number of requests for last log write

    -   \Storage Replica Partition I/O Statistics(*)\Avg. Flush Queue Length

    -   \Storage Replica Partition I/O Statistics(*)\Current Flush Queue Length

    -   \Storage Replica Partition I/O Statistics(*)\Number of Application Write Requests

    -   \Storage Replica Partition I/O Statistics(*)\Avg. Number of requests per log write

    -   \Storage Replica Partition I/O Statistics(*)\Avg. App Write Latency

    -   \Storage Replica Partition I/O Statistics(*)\Avg. App Read Latency

    -   \Storage Replica Statistics(*)\Target RPO

    -   \Storage Replica Statistics(*)\Current RPO

    -   \Storage Replica Statistics(*)\Avg. Log Queue Length

    -   \Storage Replica Statistics(*)\Current Log Queue Length

    -   \Storage Replica Statistics(*)\Total Bytes Received

    -   \Storage Replica Statistics(*)\Total Bytes Sent

    -   \Storage Replica Statistics(*)\Avg. Network Send Latency

    -   \Storage Replica Statistics(*)\Replication State

    -   \Storage Replica Statistics(*)\Avg. Message Round Trip Latency

    -   \Storage Replica Statistics(*)\Last Recovery Elapsed Time

    -   \Storage Replica Statistics(*)\Number of Flushed Recovery Transactions

    -   \Storage Replica Statistics(*)\Number of Recovery Transactions

    -   \Storage Replica Statistics(*)\Number of Flushed Replication Transactions

    -   \Storage Replica Statistics(*)\Number of Replication Transactions

    -   \Storage Replica Statistics(*)\Max Log Sequence Number

    -   \Storage Replica Statistics(*)\Number of Messages Received

    -   \Storage Replica Statistics(*)\Number of Messages Sent

    如需 Windows PowerShell 中效能計數器的詳細資訊，請參閱 [Get-Counter](/powershell/module/microsoft.powershell.diagnostics/get-counter)。

3.  若要改變延展式叢集內的複寫來源和目的地，請使用下列方法：

    1.  若要在 **Redmond** 網站中將複寫來源從某個節點移到另一個節點，可使用 Move-ClusterSharedVolume Cmdlet 來移動 CSV 資源。

        ```PowerShell
        Get-ClusterSharedVolume | fl *
        Move-ClusterSharedVolume -Name "cluster disk 4" -Node sr-srv02
        ```

    2.  若要「計劃性」地將複寫方向從某個網站移到另一個網站，可使用 **Move-ClusterSharedVolume** Cmdlet 來移動 CSV 資源。

        ```PowerShell
        Get-ClusterSharedVolume | fl *
        Move-ClusterSharedVolume -Name "cluster disk 4" -Node sr-srv04
        ```

        這也會移動適用於另一個網站和節點的記錄檔和資料。

    3.  若要執行非計劃的容錯移轉且複寫方向是從某一個網站到另一個網站：在某一個網站中，同時關閉這兩個節點的電源。

        > [!NOTE]
        > 儲存空間複本會卸載目的地磁碟區。 這是原廠設定。

4.  若要變更預設 8 GB 的記錄檔大小，請在來源和目的地儲存體複本群組上使用**set-srgroup** 。   例如，若要將所有記錄檔設定為 2 GB：

    ```PowerShell
    Get-SRGroup | Set-SRGroup -LogSizeInBytes 2GB
    Get-SRGroup
    ```

5.  若要將另一對複寫的磁碟新增到現有的複寫群組，您必須確定在可用存放裝置中至少有一個額外的磁碟。 您接著可以滑鼠右鍵按一下來源磁碟，並選取 [新增複寫合作關係]。
       >[!NOTE]
       >可用的存放裝置中需要「虛設」磁碟，是因為要用於迴歸而非刻意。 容錯移轉叢集管理員先前通常支援新增更多磁碟，這在未來的版本中將會再度支援。

    使用 **Set-SRPartnership** Cmdlet 搭配 **-SourceAddVolumePartnership** 和 **-DestinationAddVolumePartnership** 參數。
6.  若要移除複寫，在任何節點上使用 `Get-SRGroup`、Get-`SRPartnership``Remove-SRGroup` 及 `Remove-SRPartnership`。

    ```PowerShell
    Get-SRPartnership | Remove-SRPartnership
    Get-SRGroup | Remove-SRGroup
    ```

    > [!NOTE]
    > 如果使用遠端管理電腦，您必須將叢集名稱指定給這些 Cmdlet，並提供這兩個 RG 名稱。

### <a name="related-topics"></a>相關主題
- [儲存體複本總覽](storage-replica-overview.md)
- [伺服器對伺服器儲存體複寫](server-to-server-storage-replication.md)
- [叢集對叢集儲存體複寫](cluster-to-cluster-storage-replication.md)
- [儲存體複本：已知問題](storage-replica-known-issues.md)
- [儲存體複本：常見問題集](storage-replica-frequently-asked-questions.md)

## <a name="see-also"></a>另請參閱
- [Windows Server 2016](../../index.yml)
- [Windows Server 2016 中的儲存空間直接存取](../storage-spaces/storage-spaces-direct-overview.md)
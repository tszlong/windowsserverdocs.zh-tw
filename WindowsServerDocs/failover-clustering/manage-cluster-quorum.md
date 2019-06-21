---
title: 設定和管理容錯移轉叢集中的仲裁
description: 如何管理 Windows Server 容錯移轉叢集中的叢集仲裁的詳細的資訊。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.openlocfilehash: bf854418e9efb7dbb5bd07ba86f29d84ba54d68a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280381"
---
# <a name="configure-and-manage-quorum"></a>設定和管理仲裁

>適用於：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

本主題提供背景和步驟來設定和管理 Windows Server 容錯移轉叢集中的仲裁。

## <a name="understanding-quorum"></a>了解仲裁

叢集仲裁取決於投票元素的數目，這些投票元素必須是供叢集用於正常啟動或繼續執行之使用中叢集成員資格的一部分。 如需詳細的說明，請參閱[了解叢集和集區仲裁的文件](../storage/storage-spaces/understand-quorum.md)。

## <a name="quorum-configuration-options"></a>仲裁設定選項

Windows Server 中的仲裁模型很有彈性。 如果您需要修改您的叢集仲裁設定，您可以使用 [設定叢集仲裁精靈] 或容錯移轉叢集 Windows PowerShell cmdlet。 如需設定仲裁的步驟和考量，請參閱本主題之後會討論的 [設定叢集仲裁](#configure-the-cluster-quorum) 。

下表列出 [設定叢集仲裁精靈] 中提供的三個仲裁設定選項。

| 選項  |描述  |
| --------- | ---------|
| [使用一般設定]     |  叢集會自動指派投票給每個節點，並動態管理節點投票。 如果適合您的叢集，並且沒有叢集共用存放裝置可用，叢集就會選取一個磁碟見證。 建議您在大部分的情況下使用此選項，因為叢集軟體會自動選擇可為您的叢集提供最高可用性的仲裁與見證設定。       |
| [新增或變更仲裁見證]     |   您可以新增、變更或移除見證資源。 您可以設定檔案共用或磁碟見證。 叢集會自動指派投票給每個節點，並動態管理節點投票。      |
| [進階仲裁設定與見證選取項目]     | 只有在您對於設定仲裁有應用程式特定或網站特定的需求時，才選取此選項。 您可以修改仲裁見證、 新增或移除節點投票，以及選擇叢集是否動態管理節點投票。 根據預設，會將投票指派給所有節點，並動態管理節點投票。        |

系統將會視您選擇的仲裁設定選項和特定的設定而定，以下列其中一種仲裁模式設定叢集：

| 模式  | 描述  |
| --------- | ---------|
| 節點多數 (不含見證)     |   只有節點具有投票。 不設定任何仲裁見證。 叢集仲裁是使用中叢集成員資格中大部分的投票節點。      |
| 節點多數 (含見證) (磁碟或檔案共用)     |   節點具有投票。 此外，仲裁見證也具有投票。 叢集仲裁是使用中叢集成員資格加上見證投票中大部分的投票節點。 仲裁見證可以是指定的磁碟見證或指定的檔案共用見證。 
| 沒有多數 (只含磁碟見證)     | 沒有節點具有投票。 只有磁碟見證具有投票。 <br>叢集仲裁是由磁碟見證的狀態來決定。 一般而言，不建議使用，也不應該選取這種模式，因為它會使得叢集中有單一失敗點。       |

下列各節會提供進階的仲裁組態設定的詳細資訊。

### <a name="witness-configuration"></a>見證設定

做為設定仲裁時的一般規則，叢集中的投票元素應該是奇數。 因此，如果叢集包含偶數個投票節點，您應該設定磁碟見證或檔案共用見證。 叢集將可承受多一個節點停止運作。 此外，如果半數的叢集節點同時停止運作或中斷連線，新增見證投票可讓叢集繼續執行。

如果所有節點都可以看到該磁碟，通常建議使用磁碟見證。 當您需要考慮使用複寫的存放裝置進行多站台災害復原時，建議使用檔案共用見證。 只有在存放裝置廠商支援從所有站台對複寫的存放裝置進行讀寫存取時，才可以使用複寫的存放裝置設定磁碟見證。 <strong>*使用儲存空間直接存取不支援的磁碟見證*</strong>。

下表提供有關仲裁見證類型的其他資訊和考量。

| 見證類型  | 描述  | 需求和建議  |
| ---------    |---------        |---------                        |
| 磁碟見證     |  <ul><li> 儲存叢集資料庫複本的固定 LUN</li><li> 最適合用於具有共用 (非複寫) 存放裝置的叢集</li>       |  <ul><li>LUN 的大小必須至少為 512 MB</li><li> 必須是叢集專用且未指派給叢集角色</li><li> 必須包含在叢集存放裝置中並通過存放裝置驗證測試</li><li> 不能是作為叢集共用磁碟區 (CSV) 的磁碟</li><li> 具有單一磁碟區的基本磁碟</li><li> 不需要有磁碟機代號</li><li> 可以使用 NTFS 或 ReFS 格式化</li><li> 可選擇性設定硬體 RAID 以提供容錯功能</li><li> 應從備份及病毒掃描中排除</li><li> 磁碟見證不支援使用儲存空間直接存取</li>|
| 檔案共用見證     | <ul><li>在執行 Windows Server 的檔案伺服器上設定的 SMB 檔案共用</li><li> 不會儲存叢集資料庫的複本</li><li> 維護僅存在於 witness.log 檔案中的叢集資訊</li><li> 最適合用於具備複寫的存放裝置的多站台叢集 </li>       |  <ul><li>必須至少有 5 MB 的可用空間</li><li> 必須專用於單一叢集，且不用來儲存使用者或應用程式資料</li><li> 必須啟用叢集名稱之電腦物件的寫入權限</li></ul><br>以下是代管檔案共用見證之檔案伺服器的其他考量：<ul><li>單一檔案伺服器可以設定多個叢集的檔案共用見證。</li><li> 檔案伺服器必須位於與叢集工作負載分開的站台上。 如果站台至站台之間網路通訊中斷時，這可讓任何叢集站台有相同的機會繼續運作。 如果檔案伺服器位於相同站台，該網站會成為主要的站台，而且是可連線檔案共用的唯一站台。</li><li> 如果虛擬機器不是位於使用檔案共用見證的同一叢集上，檔案伺服器就可以在虛擬機器上執行。</li><li> 為了獲得高可用性，可以在獨立的容錯移轉叢集上設定檔案伺服器。 </li>      |
| 雲端見證     |  <ul><li>在 Azure blob 儲存體中儲存的見證檔案</li><li> 在叢集中的所有伺服器都具有可靠網際網路連線時，建議。</li>      |  請參閱[部署的雲端見證](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)。       |

### <a name="node-vote-assignment"></a>節點投票指派

進階的仲裁設定選項，您可以選擇要指派或移除依每個節點的仲裁投票。 根據預設，所有節點都會被指派投票。 無論是否指派投票，叢集中的所有節點都會持續運作、接收叢集資料庫更新，以及可以代管應用程式。

您可能想要從特定災害復原設定中的節點移除投票。 例如，在多站台叢集中，您可以從備份站台中的節點移除投票，讓那些節點不會影響仲裁運算。 建議只針對跨站台手動容錯移轉使用此設定。 如需詳細資訊，請參閱本主題稍後的[災害復原設定的仲裁考量](#quorum-considerations-for-disaster-recovery-configurations)。

設定節點的投票可以藉由查閱**NodeWeight**共通的屬性，使用叢集節點[Get-clusternode](https://technet.microsoft.com/library/hh847268.aspx)Windows PowerShell cmdlet。 值 0 指示節點沒有設定仲裁投票。 值 1 指示已指派節點的仲裁投票，而且是由叢集來管理。 如需管理節點投票的相關詳細資訊，請參閱本主題之後會討論的 [動態仲裁管理](#dynamic-quorum-management) 。

您可以使用 [驗證叢集仲裁]  驗證測試來驗證所有叢集節點的投票指派。

#### <a name="additional-considerations-for-node-vote-assignment"></a>節點投票指派的其他考量

  - 不建議使用節點投票指派來強制使用奇數投票節點。 而是應該改為設定磁碟見證或檔案共用見證。 如需詳細資訊，請參閱 <<c0> [ 見證設定](#witness-configuration)本主題稍後的。
  - 如果啟用動態仲裁管理，只有設定可指派節點投票的節點才能夠動態指派或移除其投票。 如需詳細資訊，請參閱本主題稍後的[動態仲裁管理](#dynamic-quorum-management)。

### <a name="dynamic-quorum-management"></a>動態仲裁管理

Windows Server 2012 中的進階的仲裁設定選項，您可以選擇啟用由叢集執行動態仲裁管理。 如需如何動態仲裁運作方式的詳細資訊，請參閱 <<c0> [ 這樣的解釋](../storage/storage-spaces/understand-quorum.md#dynamic-quorum-behavior)。

使用動態仲裁管理時，叢集也可以在最後一個倖存的叢集節點上執行。 藉由動態調整仲裁多數需求，即使節點循序關機至單一節點，叢集也可以承受。

節點的叢集指派動態投票可以使用驗證**DynamicWeight**共通的屬性，使用叢集節點[Get-clusternode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode?view=win10-ps) Windows PowerShell cmdlet。 值 0 表示節點沒有仲裁投票。 值 1 表示節點具有仲裁投票。

您可以使用 [驗證叢集仲裁]  驗證測試來驗證所有叢集節點的投票指派。

#### <a name="additional-considerations-for-dynamic-quorum-management"></a>動態仲裁管理的其他考量

- 動態仲裁管理不允許叢集承受大部分投票成員同時失敗。 若要繼續執行，叢集必須一律在節點關機或失敗時具有仲裁多數。

- 如果您已經明確移除節點的投票，叢集就無法動態新增或移除該投票。
- 儲存空間直接存取啟用時，叢集只能支援兩個節點失敗。 這會說明多[集區仲裁區段](../storage/storage-spaces/understand-quorum.md)

## <a name="general-recommendations-for-quorum-configuration"></a>仲裁設定的一般建議

叢集軟體會根據設定的節點數目和共用存放裝置的可用性，為新叢集自動設定仲裁。 這通常是最適合該叢集的仲裁設定。 不過，最好在建立叢集之後先檢閱仲裁設定，然後再將叢集置入實際執行環境。 若要檢視詳細的叢集仲裁設定，您可以使用 驗證設定精靈或[Test-cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) Windows PowerShell cmdlet，來執行**驗證仲裁設定**測試。 在 [容錯移轉叢集管理員] 中，選取叢集的摘要資訊中，會顯示基本的仲裁設定，或者您可以檢閱仲裁資源的相關資訊，顯示您在執行時傳回[Get-clusterquorum](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusterquorum?view=win10-ps)Windows PowerShell cmdlet。

您可以隨時執行 [驗證仲裁設定]  測試，以驗證仲裁設定是否為叢集適用的最佳設定。 測試輸出會指出是否建議變更仲裁設定，以及設定是否為最佳設定。 如果建議變更，您可以使用 [設定叢集仲裁精靈] 來套用建議的設定。

叢集進入實際執行環境之後，除非您判定變更適用於您的叢集，否則請不要變更仲裁設定。 您可以在下列情況下考慮變更仲裁設定：

- 新增或收回節點
- 新增或移除存放裝置
- 長期節點或見證失敗
- 在多站台災害復原情況中復原叢集

如需驗證容錯移轉叢集的詳細資訊，請參閱[驗證容錯移轉叢集的硬體](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>)。

## <a name="configure-the-cluster-quorum"></a>設定叢集仲裁

您可以使用容錯移轉叢集管理員] 或 [容錯移轉叢集 Windows PowerShell cmdlet 來設定叢集仲裁設定。

> [!IMPORTANT]
> 通常最好是使用 [設定叢集仲裁精靈] 建議的仲裁設定。 只有在您判定變更適用於您的叢集時，才建議您自訂仲裁設定。 如需詳細資訊，請參閱本主題中的 [仲裁設定的一般建議](#general-recommendations-for-quorum-configuration) 。

### <a name="configure-the-cluster-quorum-settings"></a>設定叢集仲裁設定

若要完成此程序，您至少必須在每部叢集伺服器上的本機 **Administrators** 群組中具備成員資格，或具備相等的權限。 此外，您所使用的帳戶也必須是網域使用者帳戶。

> [!NOTE]
> 您可以在不停止叢集或讓叢集資源離線的情況下變更叢集仲裁設定。

### <a name="change-the-quorum-configuration-in-a-failover-cluster-by-using-failover-cluster-manager"></a>使用容錯移轉叢集管理員變更容錯移轉叢集中的仲裁設定

1. 在 [容錯移轉叢集管理員] 中，選取或指定您想要變更的叢集。
2. 與下所選取的叢集**動作**，選取**其他動作**，然後選取**設定叢集仲裁設定**。 就會顯示 [設定叢集仲裁精靈]。 選取 **\[下一步\]** 。
3. 在 [選取仲裁設定選項]  頁面上，選取三個設定選項的其中一個，然後完成該選項的步驟。 您可以在設定仲裁設定之前檢閱您的選擇。 如需選項的詳細資訊，請參閱[了解仲裁](#understanding-quorum)稍早在本主題中。

    - 若要允許叢集自動重設最適合您目前叢集設定的仲裁設定，請選取**使用一般設定**然後完成精靈。
    - 新增或變更仲裁見證，請選取**新增或變更仲裁見證**，然後完成下列步驟。 如需設定仲裁見證的相關資訊和考量，請參閱本主題較前面的[見證設定](#witness-configuration)。

      1. 在 [選取仲裁見證]  頁面上，選取一個選項來設定磁碟見證或檔案共用見證。 精靈會指示針對您的叢集所建議的見證選擇選項。

          > [!NOTE]
          > 您也可以選取 [不設定仲裁見證]  ，然後完成精靈。 如果叢集中的投票節點數目為偶數，可能不建議使用此設定。

      2. 如果您選取設定磁碟見證的選項，在 [設定存放裝置見證]  頁面上，選取您要指派為磁碟見證的存放磁碟區，然後完成精靈。
      3. 如果您選取設定檔案共用見證的選項，請在 [設定檔案共用見證]  頁面上，輸入或瀏覽至要做為見證資源的檔案共用，然後完成精靈。

    - 若要設定仲裁管理設定和要新增或變更仲裁見證，請選取**進階仲裁設定與見證選取**，然後完成下列步驟。 如需進階仲裁組態設定的相關資訊和考量，請參閱本主題較前面的[節點投票指派](#node-vote-assignment)和[動態仲裁管理](#dynamic-quorum-management)。

      1. 在 [選取投票設定]  頁面上，選取一個選項來指派投票給節點。 根據預設，所有節點都會被指派投票。 不過，在某些案例中，您只能指派投票給節點的子集。

          > [!NOTE]
          > 您也可以選取 [無節點]  。 通常不建議這個選項，因為它不允許節點參與仲裁投票，而且需要設定磁碟見證。 這個磁碟見證會成為叢集的單一失敗點。

      2. 在 [設定仲裁管理]  頁面上，您可以啟用或停用 [允許叢集動態管理節點投票的指派]  選項。 選取這個選項通常會提高叢集的可用性。 預設會啟用此選項，並強烈建議您不要停用此選項。 此選項可讓叢集在失敗的情況下繼續執行，而停用此選項時叢集就無法繼續執行。
      3. 在 [選取仲裁見證]  頁面上，選取一個選項來設定磁碟見證或檔案共用見證。 精靈會指示針對您的叢集所建議的見證選擇選項。

          > [!NOTE]
          > 您也可以選取 [不設定仲裁見證]  ，然後完成精靈。 如果叢集中的投票節點數目為偶數，可能不建議使用此設定。

      4. 如果您選取設定磁碟見證的選項，在 [設定存放裝置見證]  頁面上，選取您要指派為磁碟見證的存放磁碟區，然後完成精靈。
      5. 如果您選取設定檔案共用見證的選項，請在 [設定檔案共用見證]  頁面上，輸入或瀏覽至要做為見證資源的檔案共用，然後完成精靈。

4. 選取 **\[下一步\]** 。 確認您的選項，在 確認 頁面隨即出現，並選取**下一步**。

執行精靈之後，**摘要**頁面隨即出現，如果您想要檢視報表的精靈所執行的工作中，選取**檢視報表**。 最新的報告將保留在<em>systemroot</em> **\\叢集\\報表**名稱的資料夾**QuorumConfiguration.mht**。

> [!NOTE]
> 在您設定叢集仲裁之後，我們建議您執行 [驗證仲裁設定]  測試，以確認更新的仲裁設定。

### <a name="windows-powershell-equivalent-commands"></a>Windows PowerShell 對應的命令

下列範例示範如何使用[Set-clusterquorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum?view=win10-ps) cmdlet 和其他 Windows PowerShell cmdlet 來設定叢集仲裁。

以下範例會將叢集 *CONTOSO-FC1* 上的仲裁設定變更為不含仲裁見證的簡單節點多數設定。

```PowerShell
Set-ClusterQuorum –Cluster CONTOSO-FC1 -NodeMajority
```

以下範例會將本機叢集上的仲裁設定變更為包含見證設定的節點多數。 名稱為 *Cluster Disk 2* 的磁碟資源會設定為磁碟見證。

```PowerShell
Set-ClusterQuorum -NodeAndDiskMajority "Cluster Disk 2"
```

以下範例會將本機叢集上的仲裁設定變更為包含見證設定的節點多數。 名為的檔案共用資源 *\\ \\CONTOSO FS\\fsw*會設定為檔案共用見證。

```PowerShell
Set-ClusterQuorum -NodeAndFileShareMajority "\\fileserver\fsw"
```

以下範例會從本機叢集上的 *ContosoFCNode1* 節點移除仲裁投票。

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=0
```

以下範例會將仲裁投票新增至本機叢集上的 *ContosoFCNode1* 節點。

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=1
```

以下範例會啟用 **CONTOSO-FC1** 叢集的 *DynamicQuorum* 屬性 (如果之前已停用)：

```PowerShell
(Get-Cluster CONTOSO-FC1).DynamicQuorum=1
```

## <a name="recover-a-cluster-by-starting-without-quorum"></a>在沒有仲裁的情況下啟動以復原叢集

沒有足夠仲裁投票的叢集將不會啟動。 您應該一律在第一個步驟中確認叢集仲裁設定，並調查為什麼叢集不再有仲裁。 如果有節點停止回應，或如果無法連線多站台叢集中的主要站台，就可能會發生這個問題。 識別叢集失敗的根本原因之後，您可以使用本節中所述的修復步驟。

> [!NOTE]
> * 如果叢集服務因為仲裁遺失而停止，系統記錄檔中會出現事件識別碼 1177。
> * 您必須調查遺失叢集仲裁的原因。
> * 最好是永遠讓節點或仲裁見證變成健康狀態 (加入叢集) 而不是啟動沒有仲裁的叢集。

### <a name="force-start-cluster-nodes"></a>強制啟動叢集節點

在判定無法透過讓節點或仲裁見證變成健康狀態的方式來復原叢集之後，就必須強制叢集啟動。 強制叢集啟動會覆寫叢集仲裁組態設定，並以 **ForceQuorum** 模式啟動叢集。

當叢集沒有仲裁時強制它啟動對於多站台叢集而言可能特別有用。 請考量包含分別位於主要與備份站台 *SiteA* 和 *SiteB* 之叢集的災害復原案例。 如果 *SiteA* 出現真正的災害，站台需要花費大量時間來重新上線。 您可能想強制 *SiteB* 上線，即使它沒有仲裁。

當叢集以 **ForceQuorum** 模式啟動，而且重新取得足夠的仲裁投票之後，它會自動離開強制的狀態，然後正常運作。 因此，不需要再次正常啟動叢集。 如果叢集遺失節點且遺失仲裁，叢集就會再次離線，因為它不再處於強制狀態。 若要讓它再次上線時它沒有仲裁需要強制叢集在沒有仲裁的情況下啟動。

> [!IMPORTANT]
> * 強制啟動叢集之後，系統管理員可以完全控制叢集。
> * 叢集會使用強制啟動叢集之節點上的叢集設定，並將它複製到所有其他可用節點。
> * 如果您強制叢集在沒有仲裁的情況下啟動，則會忽略所有仲裁組態設定，同時叢集會維持在 **ForceQuorum** 模式。 這包括特定節點投票指派和動態仲裁管理設定。

### <a name="prevent-quorum-on-remaining-cluster-nodes"></a>在剩餘叢集節點上防止仲裁

在節點上強制啟動叢集之後，必須使用防止仲裁的設定啟動叢集中任何剩餘的節點。 使用防止仲裁之設定啟動的節點，會指示叢集服務加入現有的執行中叢集，而不會形成新的叢集執行個體。 這可以防止其他節點形成包含兩個競爭之執行個體的分割叢集。

當您在一些多站台災害復原案例中需要在備份網站 *SiteB*上強制啟動叢集來復原叢集時，這是必要的做法。 若要加入 *SiteB* 中強制啟動的叢集，主要網站 *SiteA* 中的節點需要以防止仲裁的方式啟動。

> [!IMPORTANT]
> 強制啟動節點上的叢集之後，建議您一律以防止仲裁的方式啟動剩餘的節點。

以下是如何使用 「 容錯移轉叢集管理員將叢集復原：

1. 在 [容錯移轉叢集管理員] 中，選取或指定您想要復原的叢集。
2. 與下所選取的叢集**動作**，選取**強制啟動叢集**。

    容錯移轉叢集管理員會強制啟動可連線之所有節點上的叢集。 叢集將使用目前的叢集設定來啟動。

> [!NOTE]
> * 若要強制叢集啟動包含您想要使用的叢集設定的特定節點上，您必須使用 Windows PowerShell cmdlet 或對等的命令列工具在此程序之後所述。 
> * 如果您使用容錯移轉叢集管理員連線到強制啟動的叢集，而且您使用 [啟動叢集服務]  動作來啟動節點，該節點會自動使用防止仲裁的設定來啟動。

#### <a name="windows-powershell-equivalent-commands-start-clusternode"></a>Windows PowerShell 對等的命令 (Start-clusternode)

以下範例示範如何使用 **Start-ClusterNode** Cmdlet 來強制啟動節點 *ContosoFCNode1*上的叢集。

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –FQ
```

或者，您可以在本機節點上輸入以下命令：

```PowerShell
Net Start ClusSvc /FQ
```

以下範例示範如何使用 **Start-ClusterNode** cmdlet 來啟動節點 *ContosoFCNode1*上已防止仲裁的叢集服務。

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –PQ
```

或者，您可以在本機節點上輸入以下命令：

```PowerShell
Net Start ClusSvc /PQ
```

## <a name="quorum-considerations-for-disaster-recovery-configurations"></a>災害復原設定的仲裁考量

本節摘要說明災害復原部署中兩個多站台叢集設定的特性及仲裁設定。 仲裁設定指導方針會視您對站台之間的工作負載是需要自動容錯移轉或手動容錯移轉而有所差異。 您的設定通常取決於組織中現有的服務等級協定 (SLA)，以提供及支援站台上失敗或災害事件中的叢集工作負載。

### <a name="automatic-failover"></a>自動容錯移轉

在此設定中，叢集包含兩個或多個可以代管叢集角色的站台。 如果任何站台上發生失敗，叢集角色預期會自動容錯移轉至其餘站台。 因此必須設定叢集仲裁，即可讓任何站台都能夠承受站台完全失敗。

下表摘要說明這項設定的考量與建議。

| 項目  | 描述  |
| ---------| ---------|
| 每個站台的節點投票數     | 應該相等       |
| 節點投票指派     |  因為所有節點都一樣重要，所以不應該移除節點投票       |
| 動態仲裁管理     |   應該啟用      |
| 見證設定     |  對於與叢集站台不同的站台，建議設定檔案共用見證。       |
| 工作負載     |  任何站台上均可設定工作負載       |

#### <a name="additional-considerations-for-automatic-failover"></a>自動容錯移轉的其他考量

- 必須在個別站台上設定檔案共用見證，來讓每個站台有相等的存活機會。 如需詳細資訊，請參閱本主題較前面的[見證設定](#witness-configuration)。

### <a name="manual-failover"></a>手動容錯移轉

在此設定中，叢集包含一個主要站台 *SiteA*和一個備份 (復原) 站台 *SiteB*。 叢集的角色是在 *SiteA*上代管。 因為叢集仲裁設定的緣故，所以當 *SiteA* 上的所有節點發生失敗時，叢集會停止運作。 在此案例中，系統管理員必須手動容錯移轉叢集服務至 *SiteB* ，並執行其他步驟來復原叢集。

下表摘要說明這項設定的考量與建議。

| 項目   |描述  |
| ---------| ---------|
| 每個站台的節點投票數     |  <ul><li> 不應該從主要站台 **SiteA** 的節點移除節點投票</li><li>應該從備份站台 **SiteB** 的節點移除節點投票</li><li>如果 **SiteA** 發生長期中斷，必須將投票指派給 **SiteB** 上的節點，以在復原時於該站台上啟用仲裁多數</li>       |
| 動態仲裁管理     |  應該啟用       |
| 見證設定     |  <ul><li>如果 **SiteA** 上的節點數目為偶數，請設定見證</li><li>如果需要見證，則請設定只有 **SiteA** 中節點可以存取的檔案共用見證或磁碟見證 (有時也稱為非對稱式磁碟見證)</li>       |
| 工作負載     |  使用慣用的擁有者來讓工作負載持續在 **SiteA** 的節點上執行       |

#### <a name="additional-considerations-for-manual-failover"></a>手動容錯移轉的其他考量

- 一開始只會在 *SiteA* 的節點中設定仲裁投票。 這是為了確保 *SiteB* 的節點狀態不會影響叢集仲裁。
- 復原步驟會視 *SiteA* 是否可以承受暫時失敗或長期失敗而改變。

## <a name="more-information"></a>詳細資訊

* [容錯移轉叢集](failover-clustering.md)
* [容錯移轉叢集 Windows PowerShell cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)
* [了解叢集和集區仲裁](../storage/storage-spaces/understand-quorum.md)
---
title: 建立容錯移轉叢集
description: 如何建立 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2016 容錯移轉叢集。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f919e69488c4f2272ddd07e535ba4e2248ddf79c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843289"
---
# <a name="create-a-failover-cluster"></a>建立容錯移轉叢集

>適用於：Windows Server 2012 R2 中，Windows Server 2012 中，Windows Server 2016

這個主題示範如何使用容錯移轉叢集管理員嵌入式管理單元或 Windows PowerShell 建立容錯移轉叢集。 這個主題涵蓋一般部署，會在 Active Directory 網域服務 (AD DS) 中建立叢集的電腦物件及其關聯的叢集角色。 如果您要部署儲存空間直接存取叢集，而是看到[部署儲存空間直接存取](../storage/storage-spaces/deploy-storage-spaces-direct.md)。

您也可以部署已中斷連結 Active Directory 的叢集。 此部署方法可讓您不需要在 AD DS 建立電腦物件所需的權限，也不要求在 AD DS 預先設定電腦物件，就可以建立容錯移轉叢集。 此選項只透過 Windows PowerShell 提供，且只建議用於特定案例。 如需詳細資訊，請參閱[部署已中斷連結 Active Directory 的叢集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))。

#### <a name="checklist-create-a-failover-cluster"></a>檢查清單：建立容錯移轉叢集

|狀態|工作|參考資料|
|:---:|---|---|
|☐|確認先決條件|[確認先決條件](#verify-the-prerequisites)|
|☐|在想要新增為叢集節點的每個伺服器上安裝容錯移轉叢集功能|[安裝容錯移轉叢集功能](#install-the-failover-clustering-feature)|
|☐|執行 [叢集驗證精靈] 來驗證設定|[驗證設定](#validate-the-configuration)|
|☐|執行 [建立叢集精靈] 來建立容錯移轉叢集|[建立容錯移轉叢集](#create-the-failover-cluster)|
|☐|建立叢集角色來裝載叢集工作負載|[建立叢集的角色](#create-clustered-roles)|

## <a name="verify-the-prerequisites"></a>確認先決條件

在您開始之前，請確認下列先決條件：

- 請確定您想要新增為叢集節點的所有伺服器都執行相同的 Windows Server 版本。
- 檢查硬體需求，確認可以支援您的設定。 如需詳細資訊，請參閱 [Failover Clustering Hardware Requirements and Storage Options](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612869(v%3dws.11))。 如果您要建立儲存空間直接存取叢集，請參閱[儲存空間直接存取硬體需求](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md)。
- 若要在叢集建立期間新增叢集存放裝置，請確定所有伺服器都可以都存取儲存體。 (您也可以在建立叢集之後新增叢集存放裝置。)
- 請確定您想要新增為叢集節點的所有伺服器都已加入同一個 Active Directory 網域。
- (選擇性) 建立組織單位 (OU)，並將您想要新增為叢集節點之伺服器的電腦帳戶移到 OU 中。 我們建議的最佳作法是將容錯移轉叢集放置在 AD DS 中它們各自的 OU 內。 這可協助您更完善地控制哪些群組原則設定或安全性範本設定會影響叢集節點。 藉由隔離各自 OU 中的叢集，也可以防止意外刪除叢集電腦物件。

此外，請確認下列帳戶需求：

- 請確定您要用來建立叢集的帳戶，是在您想要新增為叢集節點的所有伺服器上具有系統管理員權限的網域使用者。
- 請確定下列其中一項成立：
    - 建立叢集的使用者對 OU 或對構成叢集之伺服器所在的容器具有 **建立電腦物件** 權限。
    - 如果使用者沒有**建立電腦物件**權限，請要求網域系統管理員為叢集預先設置叢集電腦物件。 如需詳細資訊，請參閱[在 Active Directory 網域服務中預先設置叢集電腦物件](prestage-cluster-adds.md)。

>[!NOTE]
>如果您想要在 Windows Server 2012 R2 上建立、 已中斷連結 Active Directory 的叢集，則不適用這項需求。 如需詳細資訊，請參閱[部署已中斷連結 Active Directory 的叢集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))。

## <a name="install-the-failover-clustering-feature"></a>安裝容錯移轉叢集功能

您必須在想要新增為容錯移轉叢集節點的每部伺服器上安裝容錯移轉叢集功能。

### <a name="install-the-failover-clustering-feature"></a>安裝容錯移轉叢集功能

1. 啟動 [伺服器管理員]。
2. 在 **管理**功能表上，選取**新增角色及功能**。
3. 在 [**在您開始之前**頁面上，選取**下一步]**。
4. 在 [**選取安裝類型**頁面上，選取**角色型或功能型安裝**，然後選取**下一步]**。
5. 在 [**選取目的地伺服器**頁面上，選取您想要用來安裝此功能，然後選取的伺服器**下一步]**。
6. 在 [**選取伺服器角色**頁面上，選取**下一步]**。
7. 在 [選取功能] 頁面上，選取 [容錯移轉叢集] 核取方塊。
8. 若要安裝容錯移轉叢集管理工具，請選取**將功能加入**，然後選取**下一步**。
9. 在 **確認安裝選項**頁面上，選取**安裝**。
<br>容錯移轉叢集功能不需要重新啟動伺服器。

10. 當安裝完成時，選取**關閉**。
11. 在想要新增為容錯移轉叢集節點的每部伺服器上重複此程序。

>[!NOTE]
>安裝容錯移轉叢集功能之後，我們建議您從 Windows Update 套用最新的更新。 此外，Windows Server 2012 型容錯移轉叢集，請檢閱[的建議 hotfix 和更新的 Windows Server 2012 架構容錯移轉叢集](https://support.microsoft.com/help/2784261/recommended-hotfixes-and-updates-for-windows-server-2012-based-failove)Microsoft 支援服務文章，並安裝任何適用的更新。

## <a name="validate-the-configuration"></a>驗證設定

建立容錯移轉叢集之前，強烈建議您驗證設定，以確定硬體與硬體設定都與容錯移轉叢集相容。 只有當完整的設定通過所有驗證測試，而且所有硬體都通過叢集節點執行之 Windows Server 版本的認證，Microsoft 才會支援叢集解決方案。

>[!NOTE]
>您必須至少有兩個節點來執行所有測試。 如果您只有一個節點，許多重要的存放裝置測試就不會執行。

### <a name="run-cluster-validation-tests"></a>執行叢集驗證測試

1. 在已從遠端伺服器管理工具安裝了容錯移轉叢集管理工具的電腦上，或是在安裝了容錯移轉叢集功能的伺服器上，啟動 [容錯移轉叢集管理員]。 若要在伺服器上這樣做，請啟動 [伺服器管理員]，然後在**工具**功能表上，選取**容錯移轉叢集管理員**。
2. 在 **容錯移轉叢集管理員**窗格下方**管理**，選取**驗證設定**。
3. 在 [**在您開始前**頁面上，選取**下一步]**。
4. 在 **選取伺服器或叢集**頁面上，於**輸入名稱**方塊、 輸入 NetBIOS 名稱或您計劃新增為容錯移轉叢集節點，在伺服器的完整的網域名稱，然後選取**新增**。 對您要新增的每部伺服器重複此步驟。 若要同時新增多部伺服器，請以逗號或分號分隔名稱。 例如，以 *server1.contoso.com,  server2.contoso.com* 的格式輸入名稱。 當您完成時，請選取**下一步**。
5. 在 [**測試選項**頁面上，選取**執行所有測試 （建議）**，然後選取**下一步]**。
6. 在 [**確認**頁面上，選取**下一步]**。

    [驗證中] 頁面會顯示執行測試的狀態。
7. 在 [摘要] 頁面上，執行下列其中一項：
    
      - 如果結果指出成功完成測試，並設定適用於叢集，，和您想要立即建立叢集，請確定**立即使用經過驗證的節點來建立叢集**檢查方塊已選取，然後選取**完成**。 然後，繼續進行[建立容錯移轉叢集](#create-the-failover-cluster)程序的步驟 4。
      - 如果結果指出發生警告或失敗，請選取**檢視報表**檢視詳細資料，並判斷必須修正的問題。 請注意，特定驗證測試的警告會指示可支援此部分的容錯移轉叢集，但可能不符合建議的最佳做法。
        
        >[!NOTE]
        >如果您收到「驗證儲存空間持續保留」測試的警告，請參閱部落格文章 [Windows 容錯移轉叢集驗證警告指示您的磁碟不支援持續保留儲存空間](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) ，以了解詳細資訊。

如需硬體驗證測試的詳細資訊，請參閱 [Validate Hardware for a Failover Cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>)。

## <a name="create-the-failover-cluster"></a>建立容錯移轉叢集

若要完成此步驟，請確定您用來登入的使用者帳戶符合本主題中[確認先決條件](#verify-the-prerequisites)一節所述的需求。

1. 啟動 [伺服器管理員]。
2. 在 **工具**功能表上，選取**容錯移轉叢集管理員**。
3. 在 **容錯移轉叢集管理員**窗格下方**管理**，選取**建立叢集**。
    
    就會開啟 [建立叢集精靈]。
4. 在 [**在您開始前**頁面上，選取**下一步]**。
5. 如果**選取伺服器**頁面隨即出現，在**輸入名稱**方塊、 輸入 NetBIOS 名稱或您計劃新增為容錯移轉叢集節點，在伺服器的完整的網域名稱，然後選取**新增**。 對您要新增的每部伺服器重複此步驟。 若要同時新增多部伺服器，請以逗號或分號分隔名稱。 例如，以 *server1.contoso.com; server2.contoso.com*的格式輸入名稱。 當您完成時，請選取**下一步**。
    
    >[!NOTE]
    >如果您選擇要執行驗證之後立即建立叢集[驗證程序的組態](#validate-the-configuration)，將不會看到**選取伺服器**頁面。 已驗證的節點會自動新增至 [建立叢集精靈]，讓您不需要重新輸入。
6. 如果您之前略過驗證，就會顯示 [驗證警告]  頁面。 我們強烈建議您執行叢集驗證。 Microsoft 只支援通過所有驗證測試的叢集。 若要執行驗證測試，請選取**是**，然後選取**下一步**。 完成 [驗證設定精靈] 中所述[驗證設定](#validate-the-configuration)。
7. 在 [管理叢集的存取點] 頁面上，執行下列動作：
    
    1. 在 [叢集名稱] 方塊中，輸入您要用來管理叢集的名稱。 執行這個動作之前，請檢閱下列資訊：
        
          - 叢集建立期間，此名稱會在 AD DS 中登錄為叢集電腦物件 (也稱為「叢集名稱物件」  或「CNO」 )。 如果您指定叢集的 NetBIOS 名稱，就會在叢集節點電腦物件所在的相同位置建立 CNO。 這可以是預設的電腦容器或 OU。
          - 若要為 CNO 指定不同的位置，您可以在 [叢集名稱]  方塊中輸入 OU 的辨別名稱。 例如: *CN=ClusterName, OU=Clusters, DC=Contoso, DC=com*。
          - 如果網域系統管理員已在不同於叢集節點所在的其他 OU 中預先設置 CNO，請指定網域系統管理員提供的辨別名稱。
    2. 如果伺服器沒有設定使用 DHCP 的網路介面卡，您必須為容錯移轉叢集設定一或多個靜態 IP 位址。 選取您想要用於叢集管理的每個網路旁邊的核取方塊。 選取 **地址**欄位旁邊所選的網路，然後輸入您想要指派給叢集的 IP 位址。 此 IP 位址 (或多個位址) 會與網域名稱系統 (DNS) 的叢集名稱相關聯。
    3. 當您完成時，請選取**下一步**。
8. 檢視 [確認] 頁面上的設定。 預設會選取 [新增適合的儲存裝置到叢集]  核取方塊。 如果您想要執行下列其中一項，請取消選取此核取方塊：
    
      - 您想要稍後再設定存放裝置。
      - 您計畫透過 [容錯移轉叢集管理員] 或透過容錯移轉叢集 Windows PowerShell Cmdlet 來建立叢集儲存空間，且尚未在檔案和存放服務中建立儲存空間。 如需詳細資訊，請參閱[部署叢集儲存空間](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)。
9. 選取 [**下一步]** 建立容錯移轉叢集。
10. 在 [摘要] 頁面上，確認已順利建立容錯移轉叢集。 如果有任何警告或錯誤，請檢視摘要輸出或選取**檢視報表**若要檢視完整的報告。 選取 **完成**。
11. 若要確認已建立叢集，請確認瀏覽樹狀目錄中 [容錯移轉叢集管理員] 底下有列出叢集名稱。 您可以展開叢集名稱，然後選取 項目底下**節點**，**儲存體**或是**網路**檢視相關聯的資源。
    
    請注意它可能需要一些時間以便成功複寫 DNS 中的叢集名稱。 成功的 DNS 登錄及複寫之後，如果您選取**所有伺服器**在 [伺服器管理員] 中，叢集名稱應該列示為 「 server 含**管理能力**狀態**Online**.

建立叢集之後，您可以執行驗證叢集仲裁設定之類的工作，也可以選擇性建立叢集共用磁碟區 (CSV)。 如需詳細資訊，請參閱 <<c0> [ 集中儲存空間直接存取的了解仲裁](../storage/storage-spaces/understand-quorum.md)並[使用容錯移轉叢集的叢集共用磁碟區](failover-cluster-csvs.md)。

## <a name="create-clustered-roles"></a>建立叢集角色

建立容錯移轉叢集之後，您可以建立叢集角色來裝載叢集工作負載。

>[!NOTE]
>對於需要用戶端存取點的叢集角色，會在 AD DS 中建立虛擬電腦物件 (VCO)。 根據預設，會在相同的容器或 OU 中將叢集的所有 VCO 建立為 CNO。 請注意，在您建立叢集之後，即可將 CNO 移至任何 OU。

以下是如何建立叢集的角色：

1. 使用伺服器管理員或 Windows PowerShell，在每個容錯移轉叢集節點上安裝叢集角色所需的角色或功能。 例如，如果您想要建立叢集檔案伺服器，請在所有叢集節點上安裝檔案伺服器角色。
    
    下表顯示您可以在 [高可用性精靈] 中設定的叢集角色，以及必須依先決條件安裝的相關聯伺服器角色或功能。
    
    <table>
    <thead>
    <tr class="header">
    <th>叢集角色</th>
    <th>角色或功能先決條件</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>DFS 命名空間伺服器</td>
    <td>DFS 命名空間 (檔案伺服器角色的一部分)</td>
    </tr>
    <tr class="even">
    <td>DHCP 伺服器</td>
    <td>DHCP 伺服器角色</td>
    </tr>
    <tr class="odd">
    <td>分散式交易協調器 (DTC)</td>
    <td>None</td>
    </tr>
    <tr class="even">
    <td>檔案伺服器</td>
    <td>檔案伺服器角色</td>
    </tr>
    <tr class="odd">
    <td>泛型應用程式</td>
    <td>不適用</td>
    </tr>
    <tr class="even">
    <td>泛型指令碼</td>
    <td>不適用</td>
    </tr>
    <tr class="odd">
    <td>泛型服務</td>
    <td>不適用</td>
    </tr>
    <tr class="even">
    <td>Hyper-V 複本代理人</td>
    <td>Hyper-V 角色</td>
    </tr>
    <tr class="odd">
    <td>iSCSI 目標伺服器</td>
    <td>iSCSI 目標伺服器 (檔案伺服器角色的一部分)</td>
    </tr>
    <tr class="even">
    <td>iSNS 伺服器</td>
    <td>iSNS 伺服器服務功能</td>
    </tr>
    <tr class="odd">
    <td>訊息佇列</td>
    <td>訊息佇列服務的功能</td>
    </tr>
    <tr class="even">
    <td>其他伺服器</td>
    <td>None</td>
    </tr>
    <tr class="odd">
    <td>虛擬機器</td>
    <td>Hyper-V 角色</td>
    </tr>
    <tr class="even">
    <td>WINS 伺服器</td>
    <td>WINS 伺服器功能</td>
    </tr>
    </tbody>
    </table>
2. 在 [容錯移轉叢集管理員] 中，展開叢集名稱，以滑鼠右鍵按一下**角色**，然後選取**設定角色**。
3. 遵循 [高可用性精靈] 中的步驟建立叢集角色。
4. 若要確認已建立叢集角色，請在 [角色]窗格中確定該角色的狀態為 [執行中]。 [角色] 窗格也會指示擁有者節點。 若要測試容錯移轉，以滑鼠右鍵按一下角色，指向**移動**，然後選取**選取節點**。 在 [**移動叢集角色**] 對話方塊中，選取所要的叢集節點，然後按**確定**。 在 [擁有者節點]  欄中，確認擁有者節點已變更。

## <a name="create-a-failover-cluster-by-using-windows-powershell"></a>使用 Windows PowerShell 建立容錯移轉叢集

下列 Windows PowerShell cmdlet 會執行這個主題中前述程序相同的函式。 以單行輸入各個 Cmdlet，由於格式限制，此處可能出現自動換行而成為數行的現象。

>[!NOTE]
>您必須使用 Windows PowerShell 來建立 Windows Server 2012 R2 中的 已中斷連結 Active Directory 的叢集。 如需語法的相關資訊，請參閱 [Deploy an Active Directory-Detached Cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))。

以下範例會安裝容錯移轉叢集功能。

```PowerShell
Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
```

以下範例會在名為 *Server1* 和 *Server2*的電腦上執行所有叢集驗證測試。

```PowerShell
Test-Cluster –Node Server1, Server2
```

>[!NOTE]
>**Test-cluster** cmdlet 將結果輸出至目前的工作目錄中的記錄檔。 例如: C:\Users\<使用者名稱 > \AppData\Local\Temp。

以下範例會建立一個名為 *MyCluster* 且包含 *Server1* 和 *Server2*節點的容錯移轉叢集，指派靜態 IP 位址 *192.168.1.12*，以及將所有合格的存放裝置新增到容錯移轉叢集。

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12
```

以下範例會建立與前面範例相同的容錯移轉叢集，但不會將合格的存放裝置新增到容錯移轉叢集。

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12 -NoStorage
```

以下範例會在網域 *Contoso.com* 的 *Cluster* OU 中建立名為 *MyCluster*的叢集。

```PowerShell
New-Cluster -Name CN=MyCluster,OU=Cluster,DC=Contoso,DC=com -Node Server1, Server2
```

如需如何新增叢集角色的範例，請參閱 [Add-ClusterFileServerRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clusterfileserverrole?view=win10-ps) 和 [Add-ClusterGenericApplicationRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergenericapplicationrole?view=win10-ps)之類的主題。

## <a name="more-information"></a>詳細資訊

  - [容錯移轉叢集](failover-clustering.md)
  - [部署 HYPER-V 叢集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj863389(v%3dws.11)>)
  - [用於應用程式資料的向外延展檔案伺服器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831349(v%3dws.11)>)
  - [部署 Active Directory 中斷連結叢集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))
  - [使用客體叢集以提供高可用性](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)
  - [叢集感知更新](cluster-aware-updating.md)
  - [New-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/new-cluster?view=win10-ps)
  - [Test-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)

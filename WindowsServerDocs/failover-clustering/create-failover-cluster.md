---
title: 建立容錯移轉叢集
description: 如何建立適用于 Windows Server 2012 R2、Windows Server 2012、Windows Server 2016 和 Windows Server 2019 的容錯移轉叢集。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
manager: lizross
ms.technology: storage-failover-clustering
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 03e5f875b19a9d903f7cefa9d2174b6e60b44c13
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827811"
---
# <a name="create-a-failover-cluster"></a>建立容錯移轉叢集

>適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012

這個主題示範如何使用容錯移轉叢集管理員嵌入式管理單元或 Windows PowerShell 建立容錯移轉叢集。 這個主題涵蓋一般部署，會在 Active Directory 網域服務 (AD DS) 中建立叢集的電腦物件及其關聯的叢集角色。 如果您要部署儲存空間直接存取叢集，請改為參閱[部署儲存空間直接存取](../storage/storage-spaces/deploy-storage-spaces-direct.md)。

您也可以部署 Active Directory 卸離的叢集。 此部署方法可讓您不需要在 AD DS 建立電腦物件所需的權限，也不要求在 AD DS 預先設定電腦物件，就可以建立容錯移轉叢集。 此選項只透過 Windows PowerShell 提供，且只建議用於特定案例。 如需詳細資訊，請參閱[部署已中斷連結 Active Directory 的叢集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))。

#### <a name="checklist-create-a-failover-cluster"></a>檢查清單：建立容錯移轉叢集

| 狀態 | 工作 | 參考 |
| ---    | ---  | ---       |
| ☐    | 確認先決條件 | [驗證必要條件](#verify-the-prerequisites) |
| ☐    | 在想要新增為叢集節點的每個伺服器上安裝容錯移轉叢集功能 | [安裝容錯移轉叢集功能](#install-the-failover-clustering-feature) |
| ☐    | 執行 [叢集驗證精靈] 來驗證設定 | [驗證設定](#validate-the-configuration) |
| ☐ | 執行 [建立叢集精靈] 來建立容錯移轉叢集 | [建立容錯移轉叢集](#create-the-failover-cluster) |
| ☐ | 建立叢集角色來裝載叢集工作負載 | [建立叢集角色](#create-clustered-roles) |

## <a name="verify-the-prerequisites"></a>確認先決條件

在您開始之前，請確認下列先決條件：

- 請確定您想要新增為叢集節點的所有伺服器都執行相同的 Windows Server 版本。
- 檢查硬體需求，確認可以支援您的設定。 如需詳細資訊，請參閱[容錯移轉叢集硬體需求及存放選項](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612869(v%3dws.11))。 如果您要建立儲存空間直接存取叢集，請參閱[儲存空間直接存取硬體需求](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md)。
- 若要在叢集建立期間新增叢集存放裝置，請確定所有伺服器都可以存取存放裝置。 (您也可以在建立叢集之後新增叢集存放裝置。)
- 請確定您想要新增為叢集節點的所有伺服器都已加入同一個 Active Directory 網域。
- (選擇性) 建立組織單位 (OU)，並將您想要新增為叢集節點之伺服器的電腦帳戶移到 OU 中。 我們建議的最佳作法是將容錯移轉叢集放置在 AD DS 中它們各自的 OU 內。 這可協助您更完善地控制哪些群組原則設定或安全性範本設定會影響叢集節點。 藉由隔離各自 OU 中的叢集，也可以防止意外刪除叢集電腦物件。

此外，請確認下列帳戶需求：

- 請確定您要用來建立叢集的帳戶，是在您想要新增為叢集節點的所有伺服器上具有系統管理員權限的網域使用者。
- 請確定下列其中一項成立：
    - 建立叢集的使用者對 OU 或對構成叢集之伺服器所在的容器具有**建立電腦物件**權限。
    - 如果使用者沒有**建立電腦物件**權限，請要求網域系統管理員為叢集預先設置叢集電腦物件。 如需詳細資訊，請參閱[在 Active Directory 網域服務中預先設置叢集電腦物件](prestage-cluster-adds.md)。

> [!NOTE]
> 如果您想要在 Windows Server 2012 R2 中建立 Active Directory 卸離的叢集，則不適用這項需求。 如需詳細資訊，請參閱[部署已中斷連結 Active Directory 的叢集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))。

## <a name="install-the-failover-clustering-feature"></a>安裝容錯移轉叢集功能

您必須在想要新增為容錯移轉叢集節點的每部伺服器上安裝容錯移轉叢集功能。

### <a name="install-the-failover-clustering-feature"></a>安裝容錯移轉叢集功能

1. 啟動 [伺服器管理員]。
2. 在 [**管理**] 功能表上，選取 [**新增角色及功能**]。
3. 在 [**開始之前**] 頁面上，選取 **[下一步]** 。
4. 在 [**選取安裝類型**] 頁面上，選取 [**角色型或功能型安裝**]，然後選取 **[下一步]** 。
5. 在 [**選取目的地伺服器**] 頁面上，選取您要安裝功能的伺服器，然後選取 **[下一步]** 。
6. 在 [選取伺服器角色] 頁面上，選取 [下一步]。
7. 在 [選取功能] 頁面上，選取 [容錯移轉叢集] 核取方塊。
8. 若要安裝容錯移轉叢集管理工具，請選取 [**新增功能**]，然後選取 **[下一步]** 。
9. 在 [**確認安裝選項**] 頁面上，選取 [**安裝**]。
<br>容錯移轉叢集功能不需要重新啟動伺服器。

10. 當安裝完成時，請選取 [**關閉**]。
11. 在想要新增為容錯移轉叢集節點的每部伺服器上重複此程序。

> [!NOTE]
> 安裝容錯移轉叢集功能之後，我們建議您從 Windows Update 套用最新的更新。 此外，針對以 Windows Server 2012 為基礎的容錯移轉叢集，請參閱[建議的 Windows server 2012 型容錯移轉叢集的修補程式和更新 Microsoft 支援服務一](https://support.microsoft.com/help/2784261/recommended-hotfixes-and-updates-for-windows-server-2012-based-failove)文，並安裝適用的任何更新。

## <a name="validate-the-configuration"></a>驗證設定

建立容錯移轉叢集之前，強烈建議您驗證設定，以確定硬體與硬體設定都與容錯移轉叢集相容。 只有當完整的設定通過所有驗證測試，而且所有硬體都通過叢集節點執行之 Windows Server 版本的認證，Microsoft 才會支援叢集解決方案。

> [!NOTE]
> 您必須至少有兩個節點來執行所有測試。 如果您只有一個節點，許多重要的存放裝置測試就不會執行。

### <a name="run-cluster-validation-tests"></a>執行叢集驗證測試

1. 在已從遠端伺服器管理工具安裝了容錯移轉叢集管理工具的電腦上，或是在安裝了容錯移轉叢集功能的伺服器上，啟動 [容錯移轉叢集管理員]。 若要在伺服器上執行此動作，請啟動伺服器管理員，然後在 [**工具**] 功能表上，選取 [**容錯移轉叢集管理員**]。
2. 在 [**容錯移轉叢集管理員**] 窗格的 [**管理**] 底下，選取 [**驗證**設定]。
3. 在 [**開始之前**] 頁面上，選取 **[下一步]** 。
4. 在 [**選取伺服器或**叢集] 頁面的 [**輸入名稱**] 方塊中，輸入您計畫新增為容錯移轉叢集節點之伺服器的 NetBIOS 名稱或完整功能變數名稱，然後選取 [**新增**]。 對您要新增的每部伺服器重複此步驟。 若要同時新增多部伺服器，請以逗號或分號分隔名稱。 例如，輸入 `server1.contoso.com, server2.contoso.com`格式的名稱。 完成後，選取 **[下一步]** 。
5. 在 [**測試選項**] 頁面上，選取 [**執行所有測試（建議）** ]，然後選取 **[下一步]** 。
6. 在 [**確認**] 頁面上，選取 **[下一步]** 。

    [驗證中] 頁面會顯示執行測試的狀態。
7. 在 [摘要] 頁面上，執行下列其中一項：
    
      - 如果結果指出測試已順利完成，且設定適用于叢集，而且您想要立即建立叢集，請確定已選取 [**立即使用已驗證的節點建立**叢集] 核取方塊，然後選取 **[完成]** 。 然後，繼續進行[建立容錯移轉叢集](#create-the-failover-cluster)程序的步驟 4。
      - 如果結果指出發生警告或失敗，請選取 [ **View Report** ] 來查看詳細資料，並判斷哪些問題必須更正。 請注意，特定驗證測試的警告會指示可支援此部分的容錯移轉叢集，但可能不符合建議的最佳做法。
        
        > [!NOTE]
        > 如果您收到「驗證儲存空間持續保留」測試的警告，請參閱部落格文章 [Windows 容錯移轉叢集驗證警告指示您的磁碟不支援持續保留儲存空間](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) ，以了解詳細資訊。

如需硬體驗證測試的相關詳細資訊，請參閱[驗證容錯移轉叢集的硬體](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>)。

## <a name="create-the-failover-cluster"></a>建立容錯移轉叢集

若要完成此步驟，請確定您用來登入的使用者帳戶符合本主題中[確認先決條件](#verify-the-prerequisites)一節所述的需求。

1. 啟動 [伺服器管理員]。
2. 在 [**工具**] 功能表上，選取 [**容錯移轉叢集管理員**]。
3. 在 [**容錯移轉叢集管理員**] 窗格的 [**管理**] 底下，選取 [**建立**叢集]。
    
    就會開啟 [建立叢集精靈]。
4. 在 [**開始之前**] 頁面上，選取 **[下一步]** 。
5. 如果出現 [**選取伺服器**] 頁面，請在 [**輸入名稱**] 方塊中，輸入您計畫新增為容錯移轉叢集節點之伺服器的 NetBIOS 名稱或完整功能變數名稱，然後選取 [**新增**]。 對您要新增的每部伺服器重複此步驟。 若要同時新增多部伺服器，請以逗號或分號分隔名稱。 例如，以 *server1.contoso.com; server2.contoso.com* 的格式輸入名稱。 完成後，選取 **[下一步]** 。
    
    > [!NOTE]
    > 如果您選擇在設定[驗證](#validate-the-configuration)程式中執行驗證之後立即建立叢集，您將不會看到 [**選取伺服器**] 頁面。 已驗證的節點會自動新增至 [建立叢集精靈]，讓您不需要重新輸入。
6. 如果您之前略過驗證，就會顯示 [驗證警告] 頁面。 我們強烈建議您執行叢集驗證。 Microsoft 只支援通過所有驗證測試的叢集。 若要執行驗證測試，請選取 **[是**]，然後選取 **[下一步]** 。 依照[驗證](#validate-the-configuration)設定中所述，完成 [驗證設定] Wizard。
7. 在 [管理叢集的存取點] 頁面上，執行下列動作：
    
    1. 在 [叢集名稱] 方塊中，輸入您要用來管理叢集的名稱。 執行這個動作之前，請檢閱下列資訊：
        
          - 叢集建立期間，此名稱會在 AD DS 中登錄為叢集電腦物件 (也稱為「叢集名稱物件」 或「CNO」)。 如果您指定叢集的 NetBIOS 名稱，就會在叢集節點電腦物件所在的相同位置建立 CNO。 這可以是預設的電腦容器或 OU。
          - 若要為 CNO 指定不同的位置，您可以在 [叢集名稱] 方塊中輸入 OU 的辨別名稱。 例如： *CN=ClusterName, OU=Clusters, DC=Contoso, DC=com*。
          - 如果網域系統管理員已在不同於叢集節點所在的其他 OU 中預先設置 CNO，請指定網域系統管理員提供的辨別名稱。
    2. 如果伺服器沒有設定使用 DHCP 的網路介面卡，您必須為容錯移轉叢集設定一或多個靜態 IP 位址。 選取您想要用於叢集管理的每個網路旁邊的核取方塊。 選取所選網路旁邊的 [**位址**] 欄位，然後輸入您要指派給叢集的 IP 位址。 此 IP 位址 (或多個位址) 會與網域名稱系統 (DNS) 的叢集名稱相關聯。
    
      >[!NOTE]
      > 如果您使用的是 Windows Server 2019，則可以選擇使用叢集的分散式網路名稱。 分散式網路名稱會使用成員伺服器的 IP 位址，而不需要叢集的私人 IP 位址。 根據預設，如果 Windows 偵測到您在 Azure 中建立叢集，則會使用分散式網路名稱（因此，您不需要為叢集建立內部負載平衡器），或如果您是在內部部署環境中執行，則為一般靜態或 IP 位址。 如需詳細資訊，請參閱[分散式網路名稱](https://blogs.windows.com/windowsexperience/2018/08/14/announcing-windows-server-2019-insider-preview-build-17733/#W0YAxO8BfwBRbkzG.97)。
    
    3. 完成後，選取 **[下一步]** 。
8. 檢視 [確認] 頁面上的設定。 預設會選取 [新增適合的儲存裝置到叢集] 核取方塊。 如果您想要執行下列其中一項，請取消選取此核取方塊：
    
      - 您想要稍後再設定存放裝置。
      - 您計畫透過 [容錯移轉叢集管理員] 或透過容錯移轉叢集 Windows PowerShell Cmdlet 來建立叢集儲存空間，且尚未在檔案和存放服務中建立儲存空間。 如需詳細資訊，請參閱[部署叢集儲存空間](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)。
9. 選取 **[下一步]** 以建立容錯移轉叢集。
10. 在 [摘要] 頁面上，確認已順利建立容錯移轉叢集。 如果出現任何警告或錯誤，請查看摘要輸出或選取 [**查看報表**]，以查看完整的報表。 選取 [完成]。
11. 若要確認已建立叢集，請確認瀏覽樹狀目錄中 [容錯移轉叢集管理員] 底下有列出叢集名稱。 您可以展開叢集名稱，然後選取 [**節點**]、[**儲存體**] 或 [**網路**] 底下的專案，以查看相關聯的資源。
    
    請注意它可能需要一些時間以便成功複寫 DNS 中的叢集名稱。 成功註冊 DNS 及複寫之後，如果您選取伺服器管理員中的 [**所有伺服器**]，叢集名稱應該會列為**管理性**狀態為 [**線上**] 的伺服器。

建立叢集之後，您可以執行驗證叢集仲裁設定之類的工作，也可以選擇性建立叢集共用磁碟區 (CSV)。 如需詳細資訊，請參閱[瞭解儲存空間直接存取中的仲裁](../storage/storage-spaces/understand-quorum.md)和在[容錯移轉叢集中使用叢集共用磁片](failover-cluster-csvs.md)區。

## <a name="create-clustered-roles"></a>建立叢集角色

建立容錯移轉叢集之後，您可以建立叢集角色來裝載叢集工作負載。

>[!NOTE]
>對於需要用戶端存取點的叢集角色，會在 AD DS 中建立虛擬電腦物件 (VCO)。 根據預設，會在相同的容器或 OU 中將叢集的所有 VCO 建立為 CNO。 請注意，在您建立叢集之後，即可將 CNO 移至任何 OU。

以下是建立叢集角色的方法：

1. 使用伺服器管理員或 Windows PowerShell，在每個容錯移轉叢集節點上安裝叢集角色所需的角色或功能。 例如，如果您想要建立叢集檔案伺服器，請在所有叢集節點上安裝檔案伺服器角色。
    
    下表顯示您可以在 [高可用性精靈] 中設定的叢集角色，以及必須依先決條件安裝的相關聯伺服器角色或功能。

   | 叢集角色  | 角色或功能先決條件  |
   | ---------       | ---------                    |
   | 命名空間伺服器     |   命名空間（檔案伺服器角色的一部分）       |
   | DFS 命名空間伺服器     |  DHCP 伺服器角色       |
   | 分散式交易協調器 (DTC)     | 無        |
   | 檔案伺服器     |  檔案伺服器角色       |
   | 泛型應用程式     |  不適用       |
   | 泛型指令碼     |   不適用      |
   | 泛型服務     |   不適用      |
   | Hyper-V 複本代理人     |   Hyper-V 角色      |
   | iSCSI 目標伺服器     |    iSCSI 目標伺服器 (檔案伺服器角色的一部分)     |
   | iSNS 伺服器     |  iSNS 伺服器服務功能       |
   | 訊息佇列     |  訊息佇列服務的功能       |
   | 其他伺服器     |  無       |
   | Virtual Machine     |  Hyper-V 角色       |
   | WINS 伺服器     |   WINS 伺服器功能      |

2. 在容錯移轉叢集管理員中，展開叢集名稱，以滑鼠右鍵按一下 [**角色**]，然後選取 [**設定角色**]。
3. 遵循 [高可用性精靈] 中的步驟建立叢集角色。
4. 若要確認已建立叢集角色，請在 [角色]窗格中確定該角色的狀態為 [執行中]。 [角色] 窗格也會指示擁有者節點。 若要測試容錯移轉，請以滑鼠右鍵按一下角色，指向 [**移動**]，然後選取 [**選取節點**]。 在 [**移動叢集角色**] 對話方塊中，選取所需的叢集節點，然後選取 **[確定]** 。 在 [擁有者節點] 欄中，確認擁有者節點已變更。

## <a name="create-a-failover-cluster-by-using-windows-powershell"></a>使用 Windows PowerShell 建立容錯移轉叢集

下列 Windows PowerShell Cmdlet 會執行與本主題中前述程式相同的功能。 以單行輸入各個 Cmdlet，由於格式限制，此處可能出現自動換行而成為數行的現象。

> [!NOTE]
> 您必須使用 Windows PowerShell 在 Windows Server 2012 R2 中建立 Active Directory 卸離的叢集。 如需語法的相關詳細資訊，請參閱[部署與 Active Directory 中斷連結的叢集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))。

以下範例會安裝容錯移轉叢集功能。

```PowerShell
Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
```

以下範例會在名為 *Server1* 和 *Server2* 的電腦上執行所有叢集驗證測試。

```PowerShell
Test-Cluster –Node Server1, Server2
```

> [!NOTE]
> **測試**叢集 Cmdlet 會將結果輸出到目前工作目錄中的記錄檔。 例如： C:\Users\<username > \Appdata\local\temp。

以下範例會建立一個名為 *MyCluster* 且包含 *Server1* 和 *Server2* 節點的容錯移轉叢集，指派靜態 IP 位址 *192.168.1.12*，以及將所有合格的存放裝置新增到容錯移轉叢集。

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12
```

以下範例會建立與前面範例相同的容錯移轉叢集，但不會將合格的存放裝置新增到容錯移轉叢集。

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12 -NoStorage
```

以下範例會在網域 *Contoso.com* 的 *Cluster* OU 中建立名為 *MyCluster* 的叢集。

```PowerShell
New-Cluster -Name CN=MyCluster,OU=Cluster,DC=Contoso,DC=com -Node Server1, Server2
```

如需如何新增叢集角色的範例，請參閱 [Add-ClusterFileServerRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clusterfileserverrole?view=win10-ps) 和 [Add-ClusterGenericApplicationRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergenericapplicationrole?view=win10-ps)之類的主題。

## <a name="more-information"></a>詳細資訊

  - [容錯移轉叢集](failover-clustering.md)
  - [部署 Hyper-v 叢集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj863389(v%3dws.11)>)
  - [用於應用程式資料的向外延展檔案伺服器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831349(v%3dws.11)>)
  - [部署 Active Directory 卸離的叢集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))
  - [使用來賓叢集來獲得高可用性](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)
  - [叢集感知更新](cluster-aware-updating.md)
  - [新增-叢集](https://docs.microsoft.com/powershell/module/failoverclusters/new-cluster?view=win10-ps)
  - [測試-叢集](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)
